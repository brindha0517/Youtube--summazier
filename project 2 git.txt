import os
from googleapiclient.discovery import build
import nltk
from nltk.tokenize import sent_tokenize
from sumy.parsers.plaintext import PlaintextParser
from sumy.nlp.tokenizers import Tokenizer
from sumy.summarizers.lsa import LsaSummarizer

# YouTube API key
API_KEY = 'AIzaSyALdmnX__qx0CYNxus1hjGd8cF7LPqSuZ8'  # Replace with your API key

# Function to get video transcript
def get_video_transcript(video_id):
    youtube = build('youtube', 'v3', developerKey=API_KEY)
    
    # Fetch video details
    request = youtube.videos().list(part='contentDetails', id=video_id)
    response = request.execute()

    # Get the video duration
    video_duration = response['items'][0]['contentDetails']['duration']
    print(f'Duration of the video: {video_duration}')
    
    # Return a placeholder transcript (You will need to implement actual transcript fetching)
    return "This is a sample transcript of the video."

# Function to summarize the transcript
def summarize_transcript(transcript):
    nltk.download('punkt')
    
    # Parse the transcript
    parser = PlaintextParser.from_string(transcript, Tokenizer("english"))
    
    # Create a summarizer
    summarizer = LsaSummarizer()
    
    # Summarize the text (adjust the number of sentences as needed)
    summary = summarizer(parser.document, 3)  # Summarize to 3 sentences
    return ' '.join(str(sentence) for sentence in summary)

# Main function
def main():
    video_url = input("Enter the YouTube video URL: ")
    video_id = video_url.split('v=')[1].split('&')[0]  # Extract video ID from URL
    
    transcript = get_video_transcript(video_id)
    print("\nTranscript:")
    print(transcript)

    summary = summarize_transcript(transcript)
    print("\nSummary:")
    print(summary)

if _name_ == "_main_":
    main()