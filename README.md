from flask import Flask, render_template, request, abort
import logging
from datetime import datetime

app = Flask(__name__)

# 1. Configure a 'Hacker Log' to record intruders
logging.basicConfig(filename='intruder_alerts.log', level=logging.INFO, 
                    format='%(asctime)s - ALERT - %(message)s')

@app.route('/')
def index():
    # A normal landing page
    return "<h1>Welcome to the Corporate Portal</h1><p>Public access only.</p>"

@app.route('/admin-portal-v2-confidential')
def hidden_trap():
    """
    This is the 'Honey-Link'. 
    Real users will never see it, but bots and scanners will find it.
    """
    # Capture intruder metadata
    intruder_data = {
        "ip": request.remote_addr,
        "user_agent": request.headers.get('User-Agent'),
        "path": request.path,
        "method": request.method,
        "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }
    
    # Log the attempt (The 'Counter-Attack' of Information)
    logging.info(f"Unauthorized Access Attempt! Data: {intruder_data}")
    
    # Return a fake error or a fake login to keep them busy
    return render_template('fake_login.html'), 401

if __name__ == '__main__':
    app.run(debug=False, port=5000)
