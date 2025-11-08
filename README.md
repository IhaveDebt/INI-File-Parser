#include <stdio.h>
#include <string.h>
#include <stdlib.h>

void parseINI(const char *filename) {
    FILE *fp = fopen(filename, "r");
    if (!fp) { perror("Error opening file"); return; }

    char line[256], section[50] = "";
    while (fgets(line, sizeof(line), fp)) {
        line[strcspn(line, "\n")] = 0;
        if (line[0] == '[') {
            sscanf(line, "[%[^]]", section);
            printf("Section: %s\n", section);
        } else if (strchr(line, '=')) {
            char key[50], value[100];
            sscanf(line, "%[^=]=%s", key, value);
            printf("  %s = %s\n", key, value);
        }
    }
    fclose(fp);
}

int main(int argc, char *argv[]) {
    if (argc < 2) {
        printf("Usage: %s <file.ini>\n", argv[0]);
        return 1;
    }
    parseINI(argv[1]);
    return 0;
}
