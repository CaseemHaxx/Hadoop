# Hadoop Setup Guide

This repository provides a bash script for setting up and configuring Hadoop on your system. The script automates the installation process and performs various configuration tasks for a smooth Hadoop setup. 

## Instructions

1. Make sure you have Java installed. If not, the script will install the default-jre for you.

2. Run the script and choose from the following options:
   - **Individual Commands**: You can select and execute specific commands based on your requirements.
   - **Run All Commands**: This option runs all the commands sequentially to set up Hadoop.

3. The available commands include:
   - **Check if Java is installed**: Verifies if Java is already installed on your system.
   - **Add Hadoop user**: Creates a new Hadoop user and sets up the necessary permissions.
   - **Configure Hadoop user**: Generates SSH keys, creates directories, and downloads Hadoop.
   - **Setup Hadoop**: Moves the Hadoop installation to the appropriate location and configures environment variables.
   - **Add properties to files**: Modifies the Hadoop configuration files with essential properties.
   - **Configure Hadoop space**: Creates directories for Hadoop data storage.
   - **Run Hadoop**: Initializes the Hadoop filesystem.
   - **Start Hadoop**: Starts the Hadoop services and displays their status.
   - **Stop Hadoop**: Stops the Hadoop services.

4. After executing the commands, you will have a fully functional Hadoop setup on your system.

**Note:** Ensure that you have the necessary permissions to execute the script and make any system-level changes.

Feel free to explore, customize, and enhance this script for your specific needs.

Happy Hadoop-ing!
