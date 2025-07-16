import requests
import random
import time
import csv
from datetime import datetime
from fake_useragent import UserAgent

# روابط الصفحات المستهدفة
URLS = [
    "https://mobailak999.blogspot.com/p/adcash.htm",
    "https://mobailak999.blogspot.com/p/adcash.html"
]

# قائمة بدول محاكة
COUNTRIES = [
    "Germany", "USA", "France", "Canada", "Brazil", "UK", "Italy", "Spain",
    "India", "Australia", "Mexico", "Netherlands", "Egypt", "Japan", "Russia"
]

# ملف CSV لتسجيل النتائج
csv_file = open("visit_log.csv", mode="w", newline="", encoding="utf-8")
csv_writer = csv.writer(csv_file)
csv_writer.writerow(["Visit #", "Time", "Country", "IP (fake)", "URL"])

ua = UserAgent()

def generate_fake_ip():
    return ".".join(str(random.randint(1, 255)) for _ in range(4))

def visit_page(visit_number):
    url = random.choice(URLS)
    country = random.choice(COUNTRIES)
    ip = generate_fake_ip()
    headers = {
        "User-Agent": ua.random,
        "X-Forwarded-For": ip,
        "X-Country": country
    }

    try:
        response = requests.get(url, headers=headers, timeout=10)
        if response.status_code == 200:
            print(f"[✔] Visit #{visit_number} to {url} from {country} ({ip})")
            csv_writer.writerow([
                visit_number,
                datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
                country,
                ip,
                url
            ])
        else:
            print(f"[✘] Failed Visit #{visit_number} (Status {response.status_code})")
    except Exception as e:
        print(f"[✘] Error in Visit #{visit_number}: {e}")

# عدد الزيارات
total_visits = 10000
delay = 3  # كل 3 ثواني

for i in range(1, total_visits + 1):
    visit_page(i)
    time.sleep(delay)

csv_file.close()
print("✅ تمت جميع الزيارات، تم حفظ التقرير في visit_log.csv")
