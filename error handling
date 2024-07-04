// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ClassMarksSystem {
    struct Student {
        string name;
        uint marks;
    }

    mapping(uint => Student) private students;
    uint[] private studentIds;

    event MarksAssigned(uint studentId, string name, uint marks);
    event Topper(uint studentId, string name, uint marks);

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

    function checkMarks(uint studentId) public view returns (string memory name, uint marks) {
        Student storage s = students[studentId];
        assert(s.marks <= 100);
        return (s.name, s.marks);
    }

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
}
