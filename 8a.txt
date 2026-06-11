""" Flask server to detect emotions """
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask(__name__)

@app.route('/')
def index():
    """ Render the main application  page"""
    return render_template('index.html')

@app.route('/emotionDetector')
def emotion_detector_post():
    """ Endpoint to detect emotions based on a text
        and return formatted output
    """
    response = emotion_detector(request.args.get('textToAnalyze'))

    if response.get('dominant_emotion') is None:
        return "Invalid text! Please try again!"
    pretty_response = "For the given statement, the system response is 'anger': "
    pretty_response+=str(response.get('anger'))
    pretty_response+=", 'disgust': "
    pretty_response+=str(response.get('disgust'))
    pretty_response+=", 'fear': "
    pretty_response+=str(response.get('fear'))
    pretty_response+=", 'joy': "
    pretty_response+=str(response.get('joy'))
    pretty_response+=", 'sadness': "
    pretty_response+=str(response.get('sadness'))
    pretty_response+=". The dominant emotion is "
    pretty_response+=str(response.get('dominant_emotion'))
    return pretty_response

if __name__ == '__main__':
    app.run(debug=True)
