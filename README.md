# Reports
Process Text Files with Python Dictionaries and Upload to Running Web Service
import os
import requests
from multiprocessing import Pool

dr = "/data/feedback/"
tasks = [i for i in os.walk(dr)][0][2]

def run(task):
    if not task.endswith(".txt"): return None
    try:
        file = dr + task
        with open(file) as f:
            text = [i.strip() for i in f.readlines()]
        data = {"title": text[0], "name": text[1], "date": text[2], "feedback": text[3]}
        req = requests.post("http://{}/feedback/", json=data)
        req.raise_for_status()

    except Exception as e:
        print(task, e)

p = Pool(len(tasks))
p.map(run, tasks)
