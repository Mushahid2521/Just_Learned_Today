## Testing Software  
Test will make the good code great.   
  
- **Manual Testing vs Automated Testing:**  When separate code is write to do the testing is called automated test. Manual when we see by running the code with expected behaviour. Automatic tests are written alongside the code that we want to test by creating separate file named moduleName_test.py  
 

#### Unit Test  
To verify small and isolated part of the program is correct  
```
from module import desired_function
import unittest

class TestModuleName(unittest.TestCase):
    def test_testcaseName(self):
        testcase = ""
        expected = ""
        self.assertEqual(desired_function(testcase), expected)

    def test_anotherTestCase(self):
        testcase = ""
        expected = ""
        self.assertEqual(desired_function(testcase), expected)

unittest.main()
```    

##### Different types of assertion  
![assertions]()     

##### Edge Case:  
Input that produce Unexpected result and are found at the extreme end of the ranges of input we imagine our program will typically work with.    
  
#### White Box Testing vs Black Box Testing  
In white box testing the test creator knows how the code works that being tested.  In case of black box testing the test creator don't have the idea how the code works but have an awareness of what the program is supposed to do.   
White box testing is  likely to be biased.    
  
- If the unittest cases are written before the code is written is called black box unit testing.    


##### Integration Test  
When the Unit tested parts are checked to work correctly in the integration environment. Integration test can be of like making network request or integrate with an API or Database.   
Simulating a different database other than the actual one not to be hampered.  
  
  
##### Regression Test  
A variant of Unittest that are written as part of a debugging and troubleshooting process to verify that an issue or error has been fixed once it's been identified.     
To confirm that a recent change of the code has not adversely affected existing features by running on a full or partial selection of test cases.  
  
##### Smoke Test  
For a web service the smoke test could be running a service on the corresponding port.   
For automation it could be running with some input and the program finishes it successfully. Just checking it works or not.  
  
##### Load Test  
Check the server load when there is many request as expected and how it handles the requests. Need to simulate the traffic as expected.   

##### Test suit  
Taking together a group of test is called test suit. A good dirversity of test types can create a more robust test suite.  
  
#### Test Driven Development   
- By creating some test first make sure that you have thought about the problem.  
- Wrtiting test first lets to think where program might be failed. 
- First writing the test script and make sure it works at the end.     

  
#### Assertion  
> asssert something==something, "This is not supposed to this"  
   
#### Rase error  
> raise ValueError("Value error with a message")  
  
#### Raise vs Assertion   
Raise to check for conditions that we expect to happen duing normal execution.  
Asssert to verify situations that aren't expected but might cause our code to misbehave.  

