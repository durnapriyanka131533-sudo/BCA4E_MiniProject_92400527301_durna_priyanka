🛒 Fake Discount Offer Detector
📌 Project Description
This is a web-based application developed using Python and Flask.
It analyzes discount offers and predicts whether they are Fake or Genuine based on rules like:
•	Discount percentage
•	Suspicious keywords
•	Combo offers (e.g., Buy 1 Get 2 Free)
________________________________________
⚙️ Requirements
Before running the project, make sure the following are installed:
•	Python (version 3.x)
•	Flask library
________________________________________
📥 Installation Steps
1.	Install Python from:
https://www.python.org
2.	Open Command Prompt / Terminal and install Flask:
3.	python -m pip install flask
________________________________________
▶️ How to Run the Project
1.	Download or copy the project folder to your laptop.
2.	Open the folder in VS Code (or any editor).
3.	Open terminal inside the folder.
4.	Run the command:
5.	python app.py
6.	You will see output like:
7.	Running on http://127.0.0.1:5000/
8.	Open browser and go to:
9.	http://127.0.0.1:5000/
________________________________________
🌐 How to Open on Another Laptop
👉 Method 1 (Easy Way):
1.	Copy the full project folder using:
o	Pen drive OR
o	Share via email/Google Drive
2.	Paste it into another laptop.
3.	Install Python & Flask (same steps above).
4.	Run:
5.	python app.py
6.	Open browser:
7.	http://127.0.0.1:5000/
________________________________________
👉 Method 2 (Same WiFi Network):
1.	In your code, change:
2.	app.run(debug=True)
                 to : app.run(host="0.0.0.0", port=5000)
3.	Run project on your laptop.
4.	Find your IP address:
5.	ipconfig
6.	On another laptop (same WiFi), open:
7.	http://YOUR_IP:5000
________________________________________
📊 Features
•	Detect fake discount offers
•	Combo offer validation
•	Price scam detection
•	Confidence score
•	User-friendly interface
________________________________________
👩‍💻 Author
•	Your Name: Durna Priyanka
•	Course: BCA
________________________________________
📌 Note
This project is for educational purposes only.

