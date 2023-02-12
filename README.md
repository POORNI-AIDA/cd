#### Backend Web Development #### 


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
