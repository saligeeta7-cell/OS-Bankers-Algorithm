#include <stdio.h>
#include <stdbool.h>

int main() {
    int n, m; 
    int i, j, k;

    printf("Enter number of processes: ");
    scanf("%d", &n);

    printf("Enter number of resource types: ");
    scanf("%d", &m);

    int alloc[n][m];
    int max[n][m];
    int avail[m];

    printf("\nEnter Allocation Matrix (%dx%d):\n", n, m);
    for (i = 0; i < n; i++) {
        for (j = 0; j < m; j++) {
            scanf("%d", &alloc[i][j]);
        }
    }

    printf("\nEnter Max Matrix (%dx%d):\n", n, m);
    for (i = 0; i < n; i++) {
        for (j = 0; j < m; j++) {
            scanf("%d", &max[i][j]);
        }
    }

    printf("\nEnter Available Resources (%d):\n", m);
    for (i = 0; i < m; i++) {
        scanf("%d", &avail[i]);
    }

    int need[n][m];
    for (i = 0; i < n; i++) {
        for (j = 0; j < m; j++) {
            need[i][j] = max[i][j] - alloc[i][j];
        }
    }

    printf("\nCalculated Need Matrix:\n");
    for (i = 0; i < n; i++) {
        printf("P%d: ", i);
        for (j = 0; j < m; j++) {
            printf("%d ", need[i][j]);
        }
        printf("\n");
    }

    bool f[n];
    int ans[n], ind = 0;
    for (k = 0; k < n; k++) {
        f[k] = false; 
    }

    int y = 0;
    for (k = 0; k < n; k++) {
        for (i = 0; i < n; i++) {
            if (f[i] == false) {
                int flag = 0;
                for (j = 0; j < m; j++) {
                    if (need[i][j] > avail[j]) {
                        flag = 1; 
                        break;
                    }
                }

                if (flag == 0) {
                    ans[ind++] = i;
                    for (y = 0; y < m; y++) {
                        avail[y] += alloc[i][y];
                    }
                    f[i] = true;
                }
            }
        }
    }

    bool isSafe = true;
    for (i = 0; i < n; i++) {
        if (f[i] == false) {
            isSafe = false;
            printf("\nThe system is NOT in a safe state! Deadlock may occur.\n");
            break;
        }
    }

    if (isSafe) {
        printf("\nSystem is in a SAFE STATE.\n");
        printf("Safe Sequence is: ");
        for (i = 0; i < n - 1; i++) {
            printf(" P%d ->", ans[i]);
        }
        printf(" P%d\n", ans[n - 1]);
    }

    return 0;
}
