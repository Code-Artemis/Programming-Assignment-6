# Programming-Assignment-6

Problem Statement & Solution Overview: The goal of this assignment was to modify two GNU Coreutils programs, ls.c and whoami.c, to change their default behavior. For ls.c, I changed the program to display the long listing format without needing the -l. For whoami.c, I modified the output to display "You are: " before the username. I used GNU Coreutils 9.7 and WSL Ubuntu to configure, compile, and test both programs. I ran into a few compilation errors, but after troubleshooting and rebuilding the binaries, I was able to get both programs working correctly.

C System Mechanics & Diff Analysis: 
I modified the default formatting logic in ls.c by replacing long_format for the first terminal-dependent output. Formatting options chosen via command-line argument processing with `getopt_long()` are stored in the format_opt variable. The default format is replaced if the user supplies an option, like -C.

The geteuid() function in whoami.c returns the effective user ID, and the getpwuid() function finds the username linked to that ID. To make the software display "You are:" and then the username, I replaced the original puts() instruction with printf().

Unified git diff:
```diff
diff --git a/src/ls.c b/src/ls.c
index 1078f88..339082c 100644
--- a/src/ls.c
+++ b/src/ls.c
@@ -2312,8 +2312,7 @@ decode_switches (int argc, char **argv)
     }
 
   format = (0 <= format_opt ? format_opt
-            : ls_mode == LS_LS ? (stdout_isatty ()
-                                  ? many_per_line : one_per_line)
+            : ls_mode == LS_LS ? long_format
             : ls_mode == LS_MULTI_COL ? many_per_line
             : /* ls_mode == LS_LONG_FORMAT */ long_format);
 
diff --git a/src/whoami.c b/src/whoami.c
index 9313eaf..16c71ad 100644
--- a/src/whoami.c
+++ b/src/whoami.c
@@ -83,6 +83,7 @@ main (int argc, char **argv)
   if (!pw)
     error (EXIT_FAILURE, errno, _("cannot find name for user ID %ju"),
           (uintmax_t) uid);
-  puts (pw->pw_name);
+  /* This command will output the prefix string "You are: " followed by the logged-in username. */
+  printf ("You are: %s\n", pw->pw_name);
   return EXIT_SUCCESS;
 }
```
Pros & Cons of Solution: One advantage of modifying ls.c is that it automatically displays more detailed information, including permissions, ownership, and file sizes. This can be useful when managing files in Linux. However, changing the default behavior could break existing scripts that expect the standard, shorter output. The output is also easier to read thanks to the modified whoami.c, although programs that only need the username may be impacted.

AI Audit Reflection: For this assignment, I used ChatGPT to help me find where I needed to make changes in ls.c and whoami.c. I asked AI to help locate the formatting variables in ls.c and explain how getopt_long() handles command-line options compared to the default settings. I also used it to better understand how geteuid() and getpwuid() work together to find the username in whoami.c. AI helped me understand what the different variables and functions were doing instead of just changing the code without knowing why. I used git diff to compare my changes to the original files and had AI check for coding style issues. After running into a few compilation errors, I was able to fix them and test both programs to make sure they worked as expected.



<img width="1816" height="420" alt="image" src="https://github.com/user-attachments/assets/1efa9d68-c824-4b57-a570-c3e99d62d5ea" />

<img width="882" height="144" alt="image" src="https://github.com/user-attachments/assets/8a3f538d-1030-4c16-ba0c-ebb958bc84ec" />

<img width="774" height="124" alt="image" src="https://github.com/user-attachments/assets/29f4086d-13ab-4006-ab47-cd0ba880f353" />

<img width="1146" height="184" alt="image" src="https://github.com/user-attachments/assets/437a67a0-9c03-4b7d-9e7a-0a5538dd9a2b" />

