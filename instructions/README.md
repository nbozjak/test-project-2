# Instruction how to complete the task

1. Install docker on your preferred linux distribution.
2. Use docker to setup and run a server using Oracle Linux (docker pull oraclelinux).
3. Give the sever a hostname (hello-kf-test-srv).
4. Modify hello.sh script in the test folder to print hello from the hostname of the server it is running on.
5. Write ansible playbook which instructs the server from step 2. to download hello.sh script from this project on Github in test folder. Execute the script on that server and leave the script running for 10 seconds showing the output and then cancel the execution.
6. Commit the results multiple times while you are working on the solution.
7. Document how to reproduce the results in readme file.
8. Use github actions to test hello.sh script and put github CI badge in readme file. 
9. Create pull request when you are done.
