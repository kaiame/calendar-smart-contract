# calendar-smart-contract
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Calendar {
    struct Event {
        string title;
        uint256 date;
        string description;
        bool exists;
    }

    mapping(address => mapping(uint256 => Event)) public userEvents;
    
    event EventCreated(address indexed user, uint256 date, string title);
    event EventDeleted(address indexed user, uint256 date);
    
    function createEvent(uint256 date, string memory title, string memory description) public {
        require(!userEvents[msg.sender][date].exists, "Event already exists on this date");
        
        userEvents[msg.sender][date] = Event(title, date, description, true);
        emit EventCreated(msg.sender, date, title);
    }
    
    function getEvent(uint256 date) public view returns (string memory, uint256, string memory) {
        require(userEvents[msg.sender][date].exists, "No event found on this date");
        
        Event storage eventDetails = userEvents[msg.sender][date];
        return (eventDetails.title, eventDetails.date, eventDetails.description);
    }
    
    function deleteEvent(uint256 date) public {
        require(userEvents[msg.sender][date].exists, "No event to delete on this date");
        
        delete userEvents[msg.sender][date];
        emit EventDeleted(msg.sender, date);
    }
}
12345
