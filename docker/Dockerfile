FROM python:3.9

# Let's verify our Python version.  python:3.9 comes with python installed
RUN python --version
RUN pip --version

# Now let's copy a script from your computer to the container
# Inside the container, create a folder called /home/scripts
RUN mkdir -p /home/scripts
COPY sample_docker_script_1.py /home/scripts

# Run that script.  NOTE:  This is done 1 time when we first build our Docker Image.
# The reason you might do this is you have a script that downloads data.  
RUN python /home/scripts/sample_docker_script_1.py

# Lastly is the command we'll run whenever the Docker Image is started.
# Please note that the only way this works is to start our container this way:
# docker run -v .:/home/host
#
# What this does is it mounts the current 
CMD ["python","/home/host/sample_docker_script_2.py"]