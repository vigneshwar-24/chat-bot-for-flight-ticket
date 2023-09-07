# chat-bot-for-flight-ticket
```
import random

# Sample flight data (you can replace this with a real flight API)
flights = {
    "AA101": {"from": "New York", "to": "Los Angeles", "departure": "2023-09-15 08:00", "price": 250},
    "UA202": {"from": "Chicago", "to": "San Francisco", "departure": "2023-09-16 10:30", "price": 300},
    "DL303": {"from": "Miami", "to": "Denver", "departure": "2023-09-17 12:15", "price": 200},
}

def find_flight(from_city, to_city, date):
    for flight_number, flight_info in flights.items():
        if flight_info["from"].lower() == from_city.lower() and flight_info["to"].lower() == to_city.lower():
            return flight_number, flight_info
    return None, None

def book_flight(flight_number):
    if flight_number in flights:
        return f"Flight {flight_number} has been booked. Thank you for choosing our service!"
    else:
        return "Sorry, that flight is not available."

# Chatbot responses
def chatbot_response(user_message):
    user_message = user_message.lower()
    
    if "book" in user_message:
        from_city = input("From which city would you like to depart? ")
        to_city = input("To which city would you like to travel? ")
        date = input("Enter your departure date (YYYY-MM-DD): ")
        
        flight_number, flight_info = find_flight(from_city, to_city, date)
        
        if flight_number and flight_info:
            return f"Found a flight for you:\nFlight Number: {flight_number}\nFrom: {flight_info['from']}\nTo: {flight_info['to']}\nDeparture: {flight_info['departure']}\nPrice: ${flight_info['price']}\n\nDo you want to book this flight? (yes/no)"
        else:
            return "Sorry, no flights match your criteria."

    elif "yes" in user_message:
        flight_number = input("Enter the flight number you want to book: ")
        return book_flight(flight_number)
    
    elif "no" in user_message:
        return "Okay, let me know if you have any other questions or need assistance with anything else."
    
    else:
        return "I can help you book a flight. Just type 'book a flight' to get started."

# Main conversation loop
print("Hello! I'm your flight booking assistant.")
print("You can ask me to book a flight or ask for information about flights.")

while True:
    user_message = input("You: ")
    if user_message.lower() == "exit":
        print("Goodbye!")
        break
    response = chatbot_response(user_message)
    print("Bot:", response)
```
