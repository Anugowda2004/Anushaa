from flask import Flask, render_template
import subprocess
import datetime
import pytz
import os
import getpass

app = Flask(_name_)

@app.route('/htop')
def htop_data():
    """
    Renders a webpage with system information including:
    - Full name (replace with your actual name)
    - System username
    - Server time in IST
    - Output of the 'top' command
    """

    full_name = "Your Full Name"  # Replace with your actual name
    username = getpass.getuser()

    # Get server time in IST
    ist = pytz.timezone('Asia/Kolkata')
    now_ist = datetime.datetime.now(ist)
    server_time_ist = now_ist.strftime("%Y-%m-%d %H:%M:%S.%f")

    # Run 'top' command and capture output
    top_process = subprocess.Popen(['top', '-bn1'], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    top_output, _ = top_process.communicate()
    top_output = top_output.decode('utf-8')

    return render_template('htop.html', 
                           full_name=full_name, 
                           username=username, 
                           server_time_ist=server_time_ist, 
                           top_output=top_output)

if _name_ == '_main_':
    app.run(host='0.0.0.0', port=5000)
