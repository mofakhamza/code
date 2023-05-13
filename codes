#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

#define BUFFER_SIZE 1024

/**
 * main - Entry point of the shell program
 *
 * Return: Always 0
 */
int main(void)
{
	char *buffer;      /* Buffer to hold user input */
	size_t bufsize = BUFFER_SIZE; /* Initial buffer size */
	ssize_t characters_read; /* Number of characters read by getline */
	char *command; /* User command */

	/* Allocate memory for the buffer */
	buffer = (char *)malloc(bufsize * sizeof(char));
	if (buffer == NULL)
	{
		perror("malloc failed");
		exit(EXIT_FAILURE);
	}

	while (1)
	{
		printf("#cisfun$ ");

		/* Read input from the user */
		characters_read = getline(&buffer, &bufsize, stdin);
		if (characters_read == -1)
		{
			/* Handle end of file condition (Ctrl+D) */
			if (feof(stdin))
			{
				printf("\n");
				break;
			}
			perror("getline failed");
			exit(EXIT_FAILURE);
		}

		buffer[characters_read - 1] = '\0'; /* Remove the newline character */

		/* Tokenize the input to extract the command */
		command = strtok(buffer, " ");
		if (command == NULL)
			continue;

		/* Check if the command is executable */
		if (access(command, X_OK) == 0)
		{
			/* Fork a child process */
			pid_t child_pid = fork();
			if (child_pid == -1)
			{
				perror("fork failed");
				exit(EXIT_FAILURE);
			}
			else if (child_pid == 0)
			{
				/* Child process - execute the command */
				execve(command, NULL, NULL);
				perror("execve failed");
				exit(EXIT_FAILURE);
			}
			else
			{
				/* Parent process - wait for the child to finish */
				wait(NULL);
			}
		}
		else
		{
			/* Command not found */
			fprintf(stderr, "%s: command not found\n", command);
		}
	}

	free(buffer);
	exit(EXIT_SUCCESS);
}
