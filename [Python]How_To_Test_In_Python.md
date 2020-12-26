# Test in python
### Unit test vs Integration test
* An integration test checks that components in your application operate with each other
* A unit test checks a small component in your application

### Example of test
```python
import unittest


class TestBasic(unittest.TestCase):
    def setUp(self):
        # Load test data
        self.app = App(database='fixtures/test_basic.json')

    def test_customer_count(self):
        self.assertEqual(len(self.app.customers), 100)

    def test_existence_of_customer(self):
        customer = self.app.get_customer(id=10)
        self.assertEqual(customer.name, "Org XYZ")
        self.assertEqual(customer.address, "10 Red Road, Reading")


class TestComplexData(unittest.TestCase):
    def setUp(self):
        # load test data
        self.app = App(database='fixtures/test_complex.json')

    def test_customer_count(self):
        self.assertEqual(len(self.app.customers), 10000)

    def test_existence_of_customer(self):
        customer = self.app.get_customer(id=9999)
        self.assertEqual(customer.name, u"バナナ")
        self.assertEqual(customer.address, "10 Red Road, Akihabara, Tokyo")

if __name__ == '__main__':
    unittest.main()
```
how to run test:
```
$ python test.py
```
```
$ python -m unittest -v test
```
v means verbose  

Once you have multiple test files, as long as you follow the  `test*.py`  naming pattern, you can provide the name of the directory instead by using the  `-s`  flag and the name of the directory:
```
$ python -m unittest discover -s tests
```

### Writing integration test
Integration testing is the testing of multiple components of the application to check that they work together. Integration testing might require acting like a consumer or user of the application by :
* calling an HTTP REST API
* calling a python API
* calling a web service
* running a command line

Each of these types of integration tests can be written in the same way as a unit test, following the Input, Execute, and Assert pattern. The most significant difference is that integration tests are checking more components at once and therefore will have more side effects than a unit test. Also, integration tests will require more fixtures to be in place, like a database, a network socket, or a configuration file.

This is why it’s good practice to separate your unit tests and your integration tests. The creation of fixtures required for an integration like a test database and the test cases themselves often take a lot longer to execute than unit tests, so you may only want to run integration tests before you push to production instead of once on every commit.

A simple way to separate unit and integration tests is simply to put them in different folders:

```
project/
│
├── my_app/
│   └── __init__.py
│
└── tests/
    |
    └── unit/
    |   ├── __init__.py
    |   └── test_sum.py
    |
    └── integration/
        |
        ├── fixtures/
        |   ├── test_basic.json
        |   └── test_complex.json
        |
        ├── __init__.py
        └── test_integration.py
```


```python
import os
import time
import boto3
import csv
import requests
import json
import unittest 
TESTSUITE = 'testsuite1'

def wait2min(time_needed):
    time_elapse = 0
    while time_elapse<time_needed:
        time.sleep(10)
        time_elapse+=10
        print('Prepareing {}{} finished'.format(float(time_elapse/time_needed)*100, "%"))


def get_checksum_from_file(filename):
    if filename.endswith(".csv"):
        result = []
        with open(filename, 'r') as f:
            reader = csv.DictReader(f)
            for row in reader:
                result.append(row['cs'])
        result.sort()
        return result


def get_checksum_from_rs(filename):
    try:
        params = {'table_name':filename}
        result = requests.post("https://hmidxuiea9.execute-api.ca-central-1.amazonaws.com/dev/regression", data=json.dumps(params)).json()
        return json.loads(result['body'])
    except:
        return []

def get_row_number(table):
    try:
        params = {'table_name':table}
        result = requests.post("https://hmidxuiea9.execute-api.ca-central-1.amazonaws.com/dev/regression", data=json.dumps(params)).json()
        return int(json.loads(result['body'])[0])
    except:
        return 0

def check_identical(a,b):
    a = ' '.join(a)
    b = ' '.join(b)
    return a==b
    

class ETL_test(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        # redcap --modality cardiacmr      --modality cardiacmr --source redcap --file_type results                                 => 0100
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/redcap-export.csv  --modality cardiacmr --source redcap --file_type results --test h".format(TESTSUITE))
        # redcap-data-dictionary-zhan      --modality cardiacmr --source redcap --file_type metadata                                => 0101
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/redcap-Data-Dictionary.csv  --modality cardiacmr --source redcap --file_type metadata --test h".format(TESTSUITE))
        # datamap                          --modality datamap --source datamap --file_type results                                  => 0600
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/Subject_ID_mapping_table.csv  --modality datamap --source datamap --file_type results --test h".format(TESTSUITE))
        # Test xnat_individual (project)   --modality carotidmr --source xnat --file_type project                                   => 0200
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/projects.csv  --modality carotidmr --source xnat --file_type project --test h".format(TESTSUITE))
        # Test xnat_individual (subject)   --modality carotidmr --source xnat --file_type subject                                   => 0201
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/subjects.csv  --modality carotidmr --source xnat --file_type subject --test h".format(TESTSUITE))
        # Test xnat_individual (session)   --modality carotidmr --source xnat --file_type session                                   => 0202
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/sessions.csv  --modality carotidmr --source xnat --file_type session --test h".format(TESTSUITE))
        # Test nightingale-test-zhan       --modality nightingale --source nightingale --file_type results                          => 0300
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/results.csv  --modality nightingale --source nightingale --file_type results --test h".format(TESTSUITE))
        # Test t-biomarkers-annotation     --modality nightingale --source nightingale --file_type 'metadata_Biomarker annotations' => 0301
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/biomarker_annotations.csv  --modality nightingale --source nightingale --file_type 'metadata_Biomarker annotations' --test h".format(TESTSUITE))
        # Test t-biomarkers-annotation     --modality nightingale --source nightingale --file_type 'metadata_Tag annotations'       => 0302
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/biomarker_annotations.csv  --modality nightingale --source nightingale --file_type 'metadata_Tag annotations' --test h".format(TESTSUITE))
        
        
        cls.ecg_table_before = get_row_number('ecg_table')
        cls.cvi_42_table_before = get_row_number("cvi_42_table")
        cls.cvi_42_wave_table_before= get_row_number("cvi_42_wave_table")
        # Test ecg-job                     --modality ecg --source muse --file_type results                                         => 0400
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/MUSE_ECG_example.xml  --modality ecg --source muse --file_type results --test h".format(TESTSUITE))
        # Test cvi-42                      --modality carotidmr --source cvi42 --file_type results                                  => 0500
        os.system("./dist/uploader --study montrealproject2 --file ./regression_test/{}/input/cvi42_sample.xml  --modality carotidmr --source cvi42 --file_type results --test h".format(TESTSUITE))

        cls.tables_redshift = [
            'redcap_export',
            'redcap_merged',
            'redcap_data_dictionary',
            'datamap_csv',
            'xnat_project',
            'xnat_subject',
            'xnat_session',
            'xnat_merged',
            # 'nightingle_result_t',
            'biomarker_annotation_csv',
            # 'tag_annotation_csv',
            'federate_stage'
        ]

        wait2min(25*60)

    def test_list_int(self):
        for i in range(len(ETL_test.tables_redshift)):
            table_name = ETL_test.tables_redshift[i]
            directory = "/home/tgao/tgao2019/platform/service/utility/uploaderBi/regression_test/{}/output/{}.csv".format(TESTSUITE, table_name)
            result_from_file = get_checksum_from_file(directory)
            result_from_rs = get_checksum_from_rs(table_name)
            print('comparing {}...'.format(table_name))
            print(check_identical(result_from_file, result_from_rs))
            self.assertEqual(result_from_file, result_from_rs)

    def test_no_cs(self):
        self.ecg_table_after = get_row_number('ecg_table')
        self.cvi_42_table_after = get_row_number("cvi_42_table")
        self.cvi_42_wave_table_after = get_row_number("cvi_42_wave_table")
        print('comparing ecg_table...')
        print(self.ecg_table_after)
        print(ETL_test.ecg_table_before)
        print(self.ecg_table_after==ETL_test.ecg_table_before+70)
        self.assertEqual(self.ecg_table_after, ETL_test.ecg_table_before+70)
        print('cvi_42_table...')
        print(self.cvi_42_table_after)
        print(ETL_test.cvi_42_table_before)
        print(self.cvi_42_table_after==ETL_test.cvi_42_table_before+802)
        self.assertEqual(self.cvi_42_table_after, ETL_test.cvi_42_table_before+802)
        print('cvi_42_wave_table...')
        print(self.cvi_42_wave_table_after)
        print(ETL_test.cvi_42_wave_table_before)
        print(self.cvi_42_wave_table_after==ETL_test.cvi_42_wave_table_before)
        self.assertEqual(self.cvi_42_wave_table_after, ETL_test.cvi_42_wave_table_before+3100)

if __name__ == '__main__':
    unittest.main()
```
## Notice the classmethod setup because it only runs once before all tests
setUp runs every time before every test
setUpClass runs only once time before all tests
