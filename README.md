DreamHouse PredictionIO Recommendation Engine
---------------------------------------------

This app uses PredictionIO to provide property recommendations based on users' favorites.

Run on Heroku:

1. [Sign up for a free Heroku account](https://heroku.com/signup)
1. [Install the Heroku Toolbelt](https://toolbelt.heroku.com)
1. Deploy the PredictionIO Event Server on Heroku: [![Deploy on Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy?template=https://github.com/jamesward/pio-eventserver-heroku)
1. Create a new app in the PredictionIO Event Server:

        heroku run console app new dreamhouse -a <YOUR EVENT SERVER APP NAME>

1. Deploy the DreamHouse Web App: [![Deploy on Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy?template=https://github.com/dreamhouseapp/dreamhouse-web-app#pio)
1. Deploy the recommendation engine: [![Deploy on Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)
1. Attach your PredictionIO Event Server's Postgres to the recommendation engine app:

    Lookup the Heroku Postgres Addon ID for the Event Server's Postgres:
    
        heroku addons -a <YOUR EVENT SERVER HEROKU APP NAME>
    
    Attach the Postgres Addon to the Engine:
    
        heroku addons:attach <YOUR ADDON ID> -a <YOUR ENGINE APP NAME>

1. Train the engine and restart it:

        heroku restart -a <YOUR ENGINE APP NAME>

1. Configure the DreamHouse Web App to know where to pull recommendations from:

        heroku config:set PIO_ENGINE_URL=<URL FOR YOUR RECOMMENDATION SERVER> -a <YOUR DREAMHOUSE WEB APP NAME>

1. Check out the recommendation in the DreamHouse Web App


Run Locally:

1. Start Postgres
1. Train the app and run the server:

        source bin/env.sh && DREAMHOUSE_WEB_APP_URL=http://localhost:8200 ACCESS_KEY=<YOUR ACCESS KEY> ./sbt "runMain ServerApp"

1. Check the status of your engine:

    http://localhost:8000

1. Check out the recommendations for an item:

    curl -H "Content-Type: application/json" -d '{"contactId": "c1", "numResults": 3 }' -k http://localhost:8000/queries.json
