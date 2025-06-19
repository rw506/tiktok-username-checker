import requests, time
from itertools import product
import string

headers = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64)"}

def generate_usernames(lengths=[3,4]):
    allowed=string.ascii_lowercase+string.digits+"._"
    for length in lengths:
        for combo in product(allowed,repeat=length):
            u=''.join(combo)
            if not u.startswith('.') and not u.endswith('.') and '..' not in u:
                yield u

def check(u):
    r=requests.get(f"https://www.tiktok.com/@{u}",headers=headers,timeout=10)
    if r.status_code==200 and "Couldn't find this account" not in r.text:
        return "✅ محجوز (مب مبند)"
    elif r.status_code==404:
        return "✅ متوفر"
    else:
        return "❌ مبند أو ما قدرنا نتحقق"

for u in generate_usernames([3,4]):
    print(f"{u} → {check(u)}")
    time.sleep(1.5)
