# Roulette-Game

This simple api roulette game consists of three components Casino, Dealer, User
Rules for playing roulette
○ Dealer opens a game
○ Multiple players can bet on which number the ball will land
○ Dealer closes the game and throws the ball


Rewards
○ Players who bet on the correct number will get double the bet amount as reward
○ Other players lose the money to the casino





API

Casino:
  A casino can have more than one dealer. A dealer may have many games, but at a time a dealer can start only one game

  Register a Casino
  POST:  
    http://localhost:3000/casinos
    
    Body:
    {"casino":{"name":"test","balance":2000}}
     name-> Name of the casino  balance-> Initial balance of the casino
   
    Sample output:
        {
        "success": true,
        "casino": {
            "id": 1, //Casino Id
            "name": "test",
            "balance": 2000.0,
            "created_at": "2020-04-23T11:18:03.326Z",
            "updated_at": "2020-04-23T11:18:03.326Z"
        }
       }
       
  Recharge a Casino
    PUT:
      http://localhost:3000/casinos/<Casino_Id>/recharge
      Example: http://localhost:3000/casinos/1/recharge
      
      Body:
        {"casino":{"recharge":500}}
        
      Sample Output:  
      {
          "success": true,
          "casino": {
              "balance": 4100.0,
              "id": 1,
              "name": "test",
              "created_at": "2020-04-23T11:18:03.326Z",
              "updated_at": "2020-04-23T12:21:13.302Z"
          }
      }    
    
    
    Add a Dealer to casino:
       POST 
         http://localhost:3000/casinos/<casino_id>dealers
         http://localhost:3000/casinos/1/dealers
         
        Body:
          {"dealer":{"name":"test"}}
         
         Sample Output:
          {
            "success": true,
            "dealers": {
                "id": 2, -> Dealer_Id , this dealer id is used to start,stop,throw in the game 
                "name": "test",
                "casino_id": 1,
                "created_at": "2020-04-23T12:25:47.121Z",
                "updated_at": "2020-04-23T12:25:47.121Z"
               }
          }
      
     List all dealers in a casino
     
           http://localhost:3000/casinos/<casino_id>dealers
       Example:
            http://localhost:3000/casinos/1/dealers
            
            Sample Output:
             {
                "success": true,
                "dealers": [
                    {
                        "id": 1,
                        "name": "sarathy",
                        "casino_id": 1,
                        "created_at": "2020-04-23T11:21:05.379Z",
                        "updated_at": "2020-04-23T11:21:05.379Z"
                    },
                    {
                        "id": 2,
                        "name": "test",
                        "casino_id": 1,
                        "created_at": "2020-04-23T12:25:47.121Z",
                        "updated_at": "2020-04-23T12:25:47.121Z"
                    },
                    {
                        "id": 3,
                        "name": "test",
                        "casino_id": 1,
                        "created_at": "2020-04-23T12:28:10.810Z",
                        "updated_at": "2020-04-23T12:28:10.810Z"
                    }
                   ]
                }
                
                
  DEALER:
     A Dealer is the one who starts the game, closes the bet on the game and throws the ball on the game
     A Dealer can start only one game at a time,so no need to pass the gameids, the api will get the current game and work
     
     Start a game:
      The dealer starts a bet in a table, where users can start betting   ,
      PUT:    
      http://localhost:3000/dealers/<dealer_id>/start
      EXAMPLE
        http://localhost:3000/dealers/1/start
        
        Sample Output:
        
       {
          "success": true,
          "game": {
              "id": 4, -> game_id -> This will be used by the user to make bets
              "dealer_id": 1,
              "gamestarttime": "2020-04-23T12:33:28.210Z",
              "gameendtime": null,
              "gamestatus": "start",
              "created_at": "2020-04-23T12:33:28.210Z",
              "updated_at": "2020-04-23T12:33:28.210Z",
              "betnumber": null
          }
      }
      
      
      
      Stop a game:
        Once the dealer stops /closes the game , no user can bet 
        
        PUT:    
          http://localhost:3000/dealers/<dealer_id>/stop
       EXAMPLE
           http://localhost:3000/dealers/1/stop
         
        
        Sample Output:
         {
              "success": true,
              "game": {
                  "dealer_id": 1,
                  "gamestatus": "close",
                  "gameendtime": "2020-04-23T12:38:22.688Z",
                  "id": 4,
                  "gamestarttime": "2020-04-23T12:33:28.210Z",
                  "created_at": "2020-04-23T12:33:28.210Z",
                  "updated_at": "2020-04-23T12:38:22.690Z",
                  "betnumber": null
              }
          }
       
    Throw ball in a game:
         
       Once the dealer stop, he throws the ball
        
        PUT:    
          http://localhost:3000/dealers/<dealer_id>/throw
        EXAMPLE
           http://localhost:3000/dealers/1/throw
            
         
         Sample Output:
          {
              "success": true,
              "game": {
                  "dealer_id": 1,
                  "gamestatus": "complete",
                  "betnumber": 35, -> THE NUMBER THROWN / OR THE NUMBER THAT THE BALL IS LANDED
                  "id": 4,
                  "gamestarttime": "2020-04-23T12:33:28.210Z",
                  "gameendtime": "2020-04-23T12:38:22.688Z",
                  "created_at": "2020-04-23T12:33:28.210Z",
                  "updated_at": "2020-04-23T12:39:32.566Z"
              }
          }
          
  
  
  USER:
    A user plays in the casino
    
  REGISTER:
     POST:
       http://localhost:3000/users
     BODY:
      {"user":{"name":"TEST","balance":1500}}
      
    SAMPLE OUTPUT:
     {
          "success": true,
          "user": {
              "id": 2, -> <user_id>
              "name": "TEST",
              "balance": 1500.0,
              "casino_id": null,
              "created_at": "2020-04-23T12:43:36.995Z",
              "updated_at": "2020-04-23T12:43:36.995Z"
          }
      }
  
  ENTER A CASINO:
      
      PUT
        http://localhost:3000/users/<user_id>/enter
      Example
         http://localhost:3000/users/1/enter
         
      BODY:
      {"user":{"casinoid":1}}
         
      SAMPLE OUTPUT:
       {
        "message": "Successfully entered the casino",
        "user": {
            "casino_id": 1,
            "id": 1,
            "name": "kk",
            "balance": 1500.0,
            "created_at": "2020-04-23T11:28:47.108Z",
            "updated_at": "2020-04-23T11:29:50.089Z"
        }
      }
  LIST ALL BETTABLE GAMES:
    GET:
      http://localhost:3000/users/<user_id>/games
      
     Example:
       http://localhost:3000/users/2/games       
     
     Sample Output:
     {
      "success": true,
      "games": [
          {
              "id": 5,
              "dealer_id": 1,
              "gamestarttime": "2020-04-23T12:49:04.529Z",
              "gameendtime": null,
              "gamestatus": "start",
              "created_at": "2020-04-23T12:49:04.530Z",
              "updated_at": "2020-04-23T12:49:04.530Z",
              "betnumber": null
          }
      ]
   }
  
  
  MAKE A BET:
   
   POST:
      http://localhost:3000/users/<user_id>/bets
     
   EXAMPLE:
      http://localhost:3000/users/2/bets

    BODY:
    
    {"bet":{"game_id":5,"betamount":100,"betnumber":10}}
     
    EXAMPLE:
    
     http://localhost:3000/users/2/bets
     
    Sample Output:
    {
    "success": true,
      "bet": {
          "id": 12,
          "user_id": 2,
          "game_id": 5,
          "betnumber": 10,
          "betstatus": "active",
          "created_at": "2020-04-23T12:53:34.436Z",
          "updated_at": "2020-04-23T12:53:34.436Z",
          "betamount": 100.0,
          "amountwon": null
        }
    } 
    
   CASHOUT AFTER BETTING :
   
     GET:
     http://localhost:3000/users/<user_id>/cashout?game_id=<game_id>
     
     Example:
     http://localhost:3000/users/2/cashout?game_id=5
        
     Sample output
     {
        "success": true,
        "bets_won": [
            {
                "user_id": 2,
                "betamount": 100.0,
                "amountwon": 200.0,
                "betstatus": "won",
                "id": 12,
                "game_id": 5,
                "betnumber": 19,
                "created_at": "2020-04-23T12:53:34.436Z",
                "updated_at": "2020-04-23T12:57:28.107Z"
            }
        ],
        "bets_lost": [],
        "TotalamountWon": 200.0,
        "message": "The  amount won will be added to your balance"
    }
    
   LIST ALL CASINOS:
   
   http://localhost:3000/casinos
   
   
    Sample Output:
      {
      "success": true,
      "casinos": [
          {
              "id": 1,
              "name": "test",
              "balance": 4200.0,
              "created_at": "2020-04-23T11:18:03.326Z",
              "updated_at": "2020-04-23T12:53:34.412Z"
          }
          ]
       }
  
   RECHARGE USER BALANCE:
    PUT:
      http://localhost:3000/users/<user_id>/recharge
    
    BODY:
    {"user":{"recharge":1}}
   
    Example:
     http://localhost:3000/users/2/recharge
     
    Sample Output:
      {
    "success": true,
    "user": {
        "balance": 1601.0,
        "id": 2,
        "name": "TEST",
        "casino_id": 1,
        "created_at": "2020-04-23T12:43:36.995Z",
        "updated_at": "2020-04-23T13:05:26.405Z"
      }
      }
      
      
      
  







              
               


       
    
    
    
