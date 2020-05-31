# the usual buildbot development bootstrap with git and virtualenv  
git clone https://github.com/buildbot/buildbot  
cd buildbot  

# run a helper script which creates the virtualenv for development.  
# Virtualenv allows to install python packages without affecting  
# other parts of the system  
make virtualenv 2>%1 | tee make.virtualenv.log  

# Activate the virtualenv.  
# After this you should see (.venv) in your shell prompt  
. .venv/bin/activate  

# now we run the test suite  
trial buildbot  

# using all CPU cores within the system helps to speed everything up  
trial -j16 buildbot  

# find all tests that talk about mail  
trial -n --reporter=bwverbose buildbot | grep mail  

# run only one test module  
trial buildbot.test.unit.test_reporters_mail  

# you can also skip the virtualenv activation using make  
make trial  

# and pass options using TRIALOPTS  
make trial TRIALOPTS='-j16 buildbot'  

# or test with a specific Python version  
make trial VENV_PY_VERSION=/usr/local/bin/python3  


# entry_points  
worker/buildbot_worker.egg-info/entry_points.txt  
master/buildbot.egg-info/entry_points.txt  

