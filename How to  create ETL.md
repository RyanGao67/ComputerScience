```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

glueContext = GlueContext(SparkContext.getOrCreate())


persons = glueContext.create_dynamic_frame.from_catalog(
             database="legislators",
             table_name="persons_json")

memberships = glueContext.create_dynamic_frame.from_catalog(
                 database="legislators",
                 table_name="memberships_json")


orgs = glueContext.create_dynamic_frame.from_catalog(
           database="legislators",
           table_name="organizations_json")



orgs = orgs.drop_fields(['other_names',
                        'identifiers']).rename_field(
                            'id', 'org_id').rename_field(
                               'name', 'org_name')

'''
memberships.select_fields(['organization_id']).toDF().distinct().show()
'''

l_history = Join.apply(orgs,
                       Join.apply(persons, memberships, 'id', 'person_id'),
                       'org_id', 'organization_id').drop_fields(['person_id', 'org_id'])

dfc = l_history.relationalize("hist_root", "s3://ryantest20190914/")


for df_name in dfc.keys():
    m_df = dfc.select(df_name)
    print "Writing to Redshift table: ", df_name
    glueContext.write_dynamic_frame.from_jdbc_conf(frame = m_df,
                                                   catalog_connection = "testred20190912",
                                                   connection_options = {"dbtable": df_name, "database": "platform"},
                                                   redshift_tmp_dir = "s3://ryantest20190914/")
```
