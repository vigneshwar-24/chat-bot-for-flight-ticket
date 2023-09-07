# chat-bot-for-flight-ticket
```
import random
from PIL import Image, ImageDraw, ImageFont
import cv2

# Sample flight data (you can replace this with a real flight API)
flights = {
    "AA101": {"from": "New York", "to": "Los Angeles", "departure": "2023-09-15 08:00", "price": 250},
    "UA202": {"from": "Chicago", "to": "San Francisco", "departure": "2023-09-16 10:30", "price": 300},
    "DL303": {"from": "Miami", "to": "Denver", "departure": "2023-09-17 12:15", "price": 200},
}

def generate_ticket(ticket_data):
    # Create a blank ticket image
    ticket_image = Image.new('RGB', (600, 400), color='white')
    draw = ImageDraw.Draw(ticket_image)

    # Set up fonts
    title_font = ImageFont.truetype('arial.ttf', size=24)
    content_font = ImageFont.truetype('arial.ttf', size=16)

    # Draw ticket content

    draw.text((50, 50), f"Name: {user_name}", font=content_font, fill='black')
    draw.text((50, 80), f"Age: {user_age}", font=content_font, fill='black')
    draw.text((50, 140), f"Train Name: {ticket_data['Train_Name']}", font=content_font, fill='black')
    draw.text((50, 170), f"Train Number: {ticket_data['Train_num']}", font=content_font, fill='black')
    draw.text((50, 230), f"Timings: {ticket_data['Timings']}", font=content_font, fill='black')

    # Save the ticket image
    ticket_image.save('ticket.png')

    # Capture image from webcam
    webcam = cv2.VideoCapture(0)
    ret, frame = webcam.read()
    cv2.imwrite('photo.png', frame)
    webcam.release()

    # Open ticket and photo images
    ticket = Image.open('ticket.png')
    photo = Image.open('photo.png')

    # Resize photo to fit in the ticket
    photo = photo.resize((100, 100))

    # Create a new image with ticket and photo combined
    combined_image = Image.new('RGB', (600, 600), color='white')
    combined_image.paste(ticket, (0, 0))
    combined_image.paste(photo, (400, 50))

    # Save the combined image
    combined_image.save('final_ticket.png')



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
        user_name = input("Whats your name? ")
        user_age = input("Your age? ")
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
