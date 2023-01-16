+++
title = "Triển khai Windows Service sử dụng AWS CodeDeploy"
date = 2023
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

In this section, you will use AWS CodeDeploy to push a Windows Service onto a group of EC2 instances and have it automatically deploy, register, and start up.

Windows Service named 'Heartbeat'. It logs *heartbeat* messages to the log file periodically and also details its running state as it transitions from stopped to running and stops to make it easy for you to see AWS CodeDeploy in action when Software updates. You will not write code for this exercise, all compiled code is provided to you. Instead, you'll focus on using AWS CodeDeploy to manage deployments for your team.