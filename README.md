# ClassMarksSystem Smart Contract
## Overview
This Solidity smart contract allows for the management of student marks in a class. The contract includes functions to assign marks, check marks, and find the topper among students, with robust error handling using require(), assert(), and revert() statements.

### Description
The ClassMarksSystem contract manages student data and marks using a mapping and an array:
1. Assign Marks: Ensures valid input, checks that marks are not reassigned, and emits an event when marks are assigned.
2. Check Marks: Returns the marks of a student.
3. Find Topper: Identifies and emits the details of the student with the highest marks.
### Mapping Variables:
1. students: Maps student IDs to their respective Student structs.
2. studentIds: An array of student IDs.

### Smart Contract Code
 #### Functions
1. assignMarks :
Assigns marks to a student if the student ID is valid, the name is provided, and the marks are within the range of 0 to 100. If marks are already assigned to the student, the function reverts.
```solidity
function assignMarks(uint studentId, string memory name, uint marks) public {
    require(studentId > 0, "Invalid student ID");
    require(bytes(name).length > 0, "Name required");
    require(marks <= 100, "Marks must be between 0 and 100");

    if (students[studentId].marks > 0) {
        revert("Marks already assigned to this student");
    }

    students[studentId] = Student(name, marks);
    studentIds.push(studentId);
    emit MarksAssigned(studentId, name, marks);
}
```
2. checkMarks :
Returns the name and marks of a student based on the student ID.
```solidity
function checkMarks(uint studentId) public view returns (string memory name, uint marks) {
    Student storage s = students[studentId];
    assert(s.marks <= 100);
    return (s.name, s.marks);
}
```
3. findTopper :
Identifies and emits the details of the student with the highest marks.
```solidity
function findTopper() public {
    require(studentIds.length > 0, "No students available");

    uint topperId = studentIds[0];
    uint highestMarks = students[topperId].marks;

    for (uint i = 1; i < studentIds.length; i++) {
        uint studentId = studentIds[i];
        if (students[studentId].marks > highestMarks) {
            highestMarks = students[studentId].marks;
            topperId = studentId;
        }
    }

    emit Topper(topperId, students[topperId].name, highestMarks);
}
```
### Author:
https://github.com/DKS-2020

### License:
This project is licensed under the MIT License - see the LICENSE.md file for details



