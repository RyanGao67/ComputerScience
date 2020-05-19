[https://www.golinuxcloud.com/ldap-tutorial-for-beginners-configure-linux/](https://www.golinuxcloud.com/ldap-tutorial-for-beginners-configure-linux/)

**Entry (or object)** - One unit in an LDAP directory. Each entry is qualified by its distinguished name (DN). Here’s an example:
dn: uid=yyang,ou=sales,dc=example,dc=com  
**Attributes** - These are pieces of information associated with an entry, such as an organization’s address or employees’ phone numbers.  
**objectClass** - This is a special type of attribute. All objects in LDAP must have an objectClass attribute. The objectClass definition specifies which attributes are required for each LDAP object, and it specifies the object classes of an entry. The values of this attribute may be modified by clients, but the objectClass attribute itself cannot be removed.   

**DN(distinguished name)** is path to an entity, RDN is used to refer an entity itself  
When we frame DN, we need to user RDN to refer to the entity.  

Object Classes are building blocks of directory information tree
Object class defines the structure of a data present in directory

Every entity should have at least one object class.   
**Structural object class,**  this is used for creating entries, every entry should have one or more structural object class   
**Auxiliary object class,**   it is not used to create entities, is used to add additional attributes to an entity created with some other structural object class
**Abstract object class,**   this sits at the top of the other object class derived from it. (Top objectclass)

LDAP has oops concept implemented in its ObjectClass, an inherited objectclass will have attributes in parent object class.   
**Frequently used objectclasses:**  
**organizational unit(OU)** it is a frequently used object class mostly it's used as a containers, which contains other objectclasses under it this is usually refered with the OU attribute  

**Organization object class, generally has it's O attribute as RDN.** This is mostly used a level above organizational unit object Class, This is refered with the O attribute

**Country (C) -** This is mostly used a level above organizational object class has it's C attributes as RDN  

**PosixAccount(UID) -** Used to store information pertaining to a user. This is usually refered with the UID attribute  
**PosixGroup(CN) -** Used to store information pertaining to a user. This is usually refered with the UID attribute  

**Frequently used attributes**   
commonName(CN) - This is the primary attribute for more than 8 object classes, if an object has a CN attribute, in modify/add/search operations the object is refered with this attribute  
Userid(UID) - This is another important attribute, used at last level of directory structure. This attribute is used by around four object classes  

Any directory structure will start from its base DN. It is the base DN where we can create our first entity. This base DN will be created during LDAP configuration

When configuring LDAP, we will be asked for public DNS
ec2-34-209.us-west-2.compute.amazonaws.com wach of the value will have a entity using domain object class. has dc(domain component) attribute as RDN
Bse Distinguished Name: dc=ec2-34-209-24-204, dc=we-west-2,dc=compute, dc=amazonaws, dc=com
