# hello-world
kho luu chư dùng để thực hành
import requests
from bs4 import BeautifulSoup
from pytube import YouTube

# URL trending YouTube
TRENDING_URL = "https://www.youtube.com/feed/trending"

def get_trending_videos():
    response = requests.get(TRENDING_URL)
    soup = BeautifulSoup(response.text, "html.parser")

    videos = []
    for link in soup.find_all("a"):
        href = link.get("href")
        if href and "/watch?v=" in href:
            full_url = "https://www.youtube.com" + href
            if full_url not in videos:
                videos.append(full_url)

    return videos[:5]  # lấy 5 video đầu

def download_video(url):
    try:
        yt = YouTube(url)
        stream = yt.streams.get_highest_resolution()
        print(f"Đang tải: {yt.title}")
        stream.download()
        print("Tải xong!\n")
    except Exception as e:
        print(f"Lỗi: {e}")

if __name__ == "__main__":
    trending_videos = get_trending_videos()
    
    for video_url in trending_videos:
        download_video(video_url)
