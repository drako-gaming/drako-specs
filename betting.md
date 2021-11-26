# Betting

The betting game goes through these stages:

- When a new bet is created, it's status is "Open". Bets can be taken while the bet is open.
- When bets are closed, the status moves to "Closed".
- When a winner is chosen, the status moves to "Done".
- A bet can be cancelled at any time. Any bets are refunded when a bet is cancelled.

## Opening the betting

Admin view: Before opening, the admin should be shown input fields to capture the objective and the options. At least two options must be entered. When the button to open the betting is clicked, the API call below is used to instruct the backend to open the betting, and the admin view changes to show how many scales are in the pot.

User view: Before opening the betting, the user is shown a message saying that there isn't a betting round open, and the results of the previous bet if applicable. After the bets are opened, the user is shown each betting option that they can select. The user is also shown an input where they can enter the maount that they want to bet. After their bet is placed, they are shown their bet, as well as the total in the pot.

Request:

```
POST /api/betting
Content-Type: application/json

{
    "objective": "Will Catmando become a dog?",
    "options": [
        {
            "description": "He will become a dog"
        },
        {
            "description": "He will not become a dog and live"
        },
        {
            "description": "He will die"
        }
    ]
}
```

Response (also pushed to everyone logged in via SignalR):

```
{
  "id": "101",
  "objective": "Will Catmando become a dog?",
  "options": [
    {
      "description": "He will become a dog",
      "id": "512",
      "total": "0"
    },
    {
      "description": "He will not become a dog and live",
      "id": "513",
      "total": "0"
    },
    {
      "description": "He will die",
      "id": "514",
      "total": "0"
    }
  ],
  "status": "Open",
  "winningOption": null,
  "total": "0",
  "alreadyBet": false
}
```

## Placing a bet

TODO

## Close the betting

Admin view: After the bets are closed, the splits for each option is shown. A button appears that when clicked, will show a view that allows the winner to be chosen.

User view: After the bets are closed, the splits for each option is shown, together with information about the bet that they placed.

Request:

```
PATCH /api/betting/:id
Content-Type: application/json

{
    "status": "Closed"
}
```

Note: `:id` in the request path is the id returned when the bet is opened.

Response (also pushed to everyone logged in via SignalR):

```
{
  "id": "101",
  "objective": "Will Catmando become a dog?",
  "options": [
    {
      "description": "He will become a dog",
      "id": "512",
      "total": "300"
    },
    {
      "description": "He will not become a dog and live",
      "id": "513",
      "odds": null,
      "total": "400"
    },
    {
      "description": "He will die",
      "id": "514",
      "total": "0"
    }
  ],
  "status": "Open",
  "winningOption": null,
  "total": "700",
  "alreadyBet": false
}
```

## Choose the winner

Admin view: The admin is shown a view that allows them to choose the winning option. When the option is chosen, a list of the wieners are shown, together with how much they bet and how much they won.

User view: When the wiener is chosen, They are shown how much they won.

Request:

```
PATCH /api/betting/:id
Content-Type: application/json

{
    "status": "Done",
    "winningOption": ":optionId"
}
```

Note: `:id` in the request path is the id returned when the bet is opened and `:optionId` is the ID of the option that is picked as the winner.

Response (also pushed to everyone logged in via SignalR):

```
{
  "id": "101",
  "objective": "Will Catmando become a dog?",
  "options": [
    {
      "description": "He will become a dog",
      "id": "512",
      "total": "300"
    },
    {
      "description": "He will not become a dog and live",
      "id": "513",
      "odds": null,
      "total": "400"
    },
    {
      "description": "He will die",
      "id": "514",
      "total": "0"
    }
  ],
  "status": "Done",
  "winningOption": "513",
  "total": "700",
  "alreadyBet": false
}
```

Additional APIs (TODO):

- Get the list of bets by game id
- Get the most recent open bet
