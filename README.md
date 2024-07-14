# Website-programming1
### Task: Build a user interface for converting audio to text (speech to text) And save the text output to the database


#### Created a database in php
```
CREATE DATABASE data_outpt;

USE data_outpt;

CREATE TABLE your_table_name (
    id INT AUTO_INCREMENT PRIMARY KEY,
    text TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Create a web page for Speech to Text Convert
```
<!DOCTYPE html>
<html>
<head>
  <title>Speech to Text(Arabic)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
      color: blue;
    }
    #text-output {
      width: 80%;
      height: 200px;
      font-size: 16px;
      padding: 10px;
      display: block;
      margin: 20px auto;
      background-color: #f2f2f2;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
    button {
      font-size: 16px;
      padding: 10px 20px;
      background-color: pink;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Speech to Text</h1>
  <textarea id="text-output" placeholder="Converted text will appear here..."></textarea>
  <button onclick="convertSpeechToText()">Convert Speech to Text</button>

  <script>
    function convertSpeechToText() {
      // Check if the browser supports the Web Speech API
      if ('webkitSpeechRecognition' in window) {
        var recognition = new webkitSpeechRecognition();
        recognition.lang = 'ar-SA'; // Set the language to Arabic
        recognition.onresult = function(event) {
          var text = event.results[0][0].transcript;
          document.getElementById('text-output').value = text;
          
          // Save the text to a database
          saveTextToDatabase(text);
        };
        recognition.start();
      } else {
        alert('Speech recognition is not supported in this browser.');
      }
    }

    function saveTextToDatabase(text) {
      var xhr = new XMLHttpRequest();
      xhr.open("POST", "save_text.php", true);
      xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
          alert("Text saved to database.");
        }
      };
      xhr.send("text=" + encodeURIComponent(text));
    }
  </script>
</body>
</html>
```

#### Create a command code to save the data in the created database
```
<?php
$servername = "localhost";
$username = "root";
$password = ""; // Add your database password if you have one
$dbname = "data_outpt";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $text = $_POST['text'];
    
    $stmt = $conn->prepare("INSERT INTO your_table_name (text) VALUES (?)");
    $stmt->bind_param("s", $text);
    
    if ($stmt->execute()) {
        echo "Text saved successfully";
    } else {
        echo "Error: " . $stmt->error;
    }
    
    $stmt->close();
}

$conn->close();
?>
```
<img src="https://github.com/Samar-Hamed2003/Website-programming/assets/173670288/764d0762-bd77-4caf-8d08-335efd2bd34d.jpg" width="50%" height="50%">

