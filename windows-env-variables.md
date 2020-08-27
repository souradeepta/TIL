## [How can I display the contents of an environment variable from the command prompt in Windows 7?](https://superuser.com/questions/341192/how-can-i-display-the-contents-of-an-environment-variable-from-the-command-promp)

- In Windows Command-Prompt the syntax is `echo %PATH%`

- To get a list of all environment variables enter the command `set`

- To send those variables to a text file enter the command `set > filename.txt`

- if you're using Powershell `echo %PATH%` would not work. You need to use the following command instead: `echo $Env:PATH`

- Also in PS: `ls env:` for listing all environment variables

- To display all variables starting with "h", type: `set h`

-  While *SET* works with global or system variables, sometimes you want to write and read User variables, and this is done with the *SETX* command. SETX is included in the base installs of Windows beginning with Vista, but was also available in Windows XP by installing the Resource Pack.

- **To Set a \*user\* variable using SETX:**

  ```
  setx variable value
  ```

  **To set a global or system variable using SETX:**

  ```
  setx /m variable value
  ```

  **To read a user or global variable:**

  *Remember, you must open a new Command or Powershell window to read this variable.*

  ```
  echo %variable%
  ```

