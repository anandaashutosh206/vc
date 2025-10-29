// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract VoteBank {
    // --- STRUCTS ---
    struct Candidate {
        string name;
        uint voteCount;
    }

    // --- STATE VARIABLES ---
    address public owner;
    bool public votingOpen;
    mapping(address => bool) public hasVoted;
    Candidate[] public candidates;

    // --- EVENTS ---
    event CandidateAdded(string name);
    event Voted(address voter, uint candidateIndex);
    event VotingOpened();
    event VotingClosed();

    // --- MODIFIERS ---
    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this");
        _;
    }

    modifier votingActive() {
        require(votingOpen, "Voting is not open");
        _;
    }

    // --- CONSTRUCTOR ---
    constructor() {
        owner = msg.sender;
    }

    // --- FUNCTIONS ---

    // Add a candidate (only by owner, before voting starts)
    function addCandidate(string memory _name) public onlyOwner {
        require(!votingOpen, "Cannot add candidates once voting starts");
        candidates.push(Candidate(_name, 0));
        emit CandidateAdded(_name);
    }

    // Open voting (owner only)
    function openVoting() public onlyOwner {
        require(!votingOpen, "Voting already open");
        require(candidates.length > 0, "Add candidates first");
        votingOpen = true;
        emit VotingOpened();
    }

    // Close voting (owner only)
    function closeVoting() public onlyOwner {
        require(votingOpen, "Voting already closed");
        votingOpen = false;
        emit VotingClosed();
    }

    // Vote for a candidate by index
    function vote(uint candidateIndex) public votingActive {
        require(!hasVoted[msg.sender], "You already voted");
        require(candidateIndex < candidates.length, "Invalid candidate index");

        hasVoted[msg.sender] = true;
        candidates[candidateIndex].voteCount += 1;

        emit Voted(msg.sender, candidateIndex);
    }

    // Get total number of candidates
    function getCandidateCount() public view returns (uint) {
        return candidates.length;
    }

    // Get candidate details
    function getCandidate(uint index) public view returns (string memory name, uint votes) {
        require(index < candidates.length, "Invalid index");
        Candidate memory c = candidates[index];
        return (c.name, c.voteCount);
    }
}
