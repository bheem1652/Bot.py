from iqoptionapi.stable_api import IQ_Option
import time

# Login credentials (अपना ईमेल और पासवर्ड डालो)
email = "your_email@example.com"
password = "your_password"

# Initialize API
Iq = IQ_Option(email, password)
Iq.connect()

# Check connection
if Iq.check_connect():
    print("Connected successfully!")
else:
    print("Connection failed!")
    exit()

# Trading parameters
asset = "EURUSD"
investment = 1  # Amount per trade
duration = 1  # Trade duration in minutes

def place_trade(direction):
    status, trade_id = Iq.buy(investment, asset, direction, duration)
    if status:
        print(f"Trade placed successfully! Direction: {direction}")
    else:
        print("Trade placement failed!")

# Simple strategy (Random buy/sell every minute)
while True:
    current_time = time.localtime()
    if current_time.tm_sec == 0:  # Execute trade at the start of every minute
        direction = "call" if current_time.tm_min % 2 == 0 else "put"
        place_trade(direction)
        time.sleep(60)  # Wait for a minute before placing next trade
