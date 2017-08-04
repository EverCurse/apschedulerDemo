#!/usr/bin/env python
# __*__ coding: utf-8 __*__
import random
from flask import Flask,jsonify
from flask_apscheduler import APScheduler
from datetime import date,datetime
from apscheduler.jobstores.sqlalchemy import SQLAlchemyJobStore
from apscheduler.jobstores.redis import RedisJobStore

app = Flask(__name__)

class Config(object):
    SCHEDULER_JOBSTORES = {
        #'default': SQLAlchemyJobStore(url='sqlite:///9.db;'),
        'default': RedisJobStore(host='10.157.60.133',port=6379,jobs_key='apscheduler.jobs', run_times_key='apscheduler.run_times')
    }
    SCHEDULER_API_ENABLED = True
app.config.from_object(Config())

def myJob():
    print 'xxxxxxxxxxxxxxxxxxxxx'

def myJob2():
    print 'yyyyyyyyyyyyyyyy'

@app.route('/jobs')
def jobs():
    print sche.get_jobs()
    return "jobs"

@app.route('/add')
def add():
    id = str(datetime.now().strftime("%H-%M-%S"))
    sche.add_job(func=myJob, id=id, trigger='interval', seconds=5)
    sche.add_job(func=myJob2, id=id+'222', trigger='cron', hour='17',minute='52')
    return 'success'
if __name__ == '__main__':
    sche = APScheduler()
    sche.init_app(app)
    sche.start()
    app.run(debug=True)
