# Test in python
### Unit test vs Integration test
* An integration test checks that components in your application operate with each other
* A unit test checks a small component in your application

### Example of test
```python
import unittest

from my_sum import sum

class TestSum(unittest.TestCase):
    def test_list_int(self):
        """
 	Test that it can sum a list of integers
 	"""
        data = [1, 2, 3]
        result = sum(data)
        self.assertEqual(result, 6)

    def test_list_fraction(self):
        """
 	Test that it can sum a list of fractions
 	"""
        data = [Fraction(1, 4), Fraction(1, 4), Fraction(2, 5)]
        result = sum(data)
        self.assertEqual(result, 1)

    def test_bad_type(self): 
	data = "banana" 
	with self.assertRaises(TypeError): 
	    result = sum(data) 
			
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
