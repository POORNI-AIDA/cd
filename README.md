For this assessment, you will have to build a REST API Ticketing System. The logic of
the system is described below:
When a new ticket comes in, the system is expected to assign the incoming ticket to a
person based on the Round Robin Principle.You can either use Python Flask or Nodejs for the development.
Create a sample static data of 5 people with the following attributes and store them
locally,
1. A unique ID (use consecutive numbers. eg ‘#1’, ‘#2’ and so on)
2. Name
3. List of tickets assigned to them
A ticket should contain the following attributes,
1. A unique ID
2. Issue description
3. Assigned to (user ID)
4. Raised by (user ID)


import json
    
from flask import Flask, request, jsonify

app = Flask(__name__)

people = [
    {
        "id": '#1',
        "name": "Jack",
        "tickets": []
    },
    {
        "id": '#2',
        "name": "Jenny",
        "tickets": []
    },
    {
        "id": '#3',
        "name": "Shreya",
        "tickets": []
    },
    {
        "id": '#4',
        "name": "Sid",
        "tickets": []
    },
    {
        "id": '#5',
        "name": "Ryan",
        "tickets": []
    }
]

ticket_id = '#1'

@app.route("/tickets", methods=["POST"])
def create_ticket():
    global ticket_id
    data = request.get_json()
    issue_description = data["issue_description"]
    raised_by = data["raised_by"]
    
    person = people[(ticket_id - 1) % len(people)]
    ticket = {
        "id": ticket_id,
        "issue_description": issue_description,
        "assigned_to": person["id"],
        "raised_by": raised_by
    }
    person["tickets"].append(ticket)
    ticket_id += 1
    
    return jsonify(ticket), 201

@app.route("/tickets/<int:ticket_id>", methods=["GET"])
def get_ticket(ticket_id):
    for person in people:
        for ticket in person["tickets"]:
            if ticket["id"] == ticket_id:
                return jsonify(ticket), 200
    return jsonify({"message": "Ticket not found"}), 404

if __name__ == "__main__":
    app.run()
