#include <stdio.h>
#include <stdlib.h>

int process_data(const char *filename) {
    FILE *fp = NULL;
    char *buffer = NULL;
    int result = -1;

    fp = fopen(filename, "r");
    if (!fp) {
        perror("fopen");
        return -1;
    }

    buffer = malloc(1024);
    if (!buffer) {
        perror("malloc");
        goto cleanup;
    }

    if (fgets(buffer, 1024, fp) == NULL) {
        perror("fgets");
        goto cleanup;
    }

    printf("data: %s\n", buffer);
    result = 0;

cleanup:
    free(buffer);
    buffer = NULL;
    if (fp) {
        fclose(fp);
        fp = NULL;
    }
    return result;
}#include <iostream>
#include <memory>
#include <vector>

struct Data {
    int value;
    Data(int v) : value(v) { std::cout << "constructed\n"; }
    ~Data() { std::cout << "destroyed\n"; }
};

void example() {
    std::unique_ptr<Data> p = std::make_unique<Data>(42);
    std::cout << p->value << "\n";

    std::vector<std::unique_ptr<Data>> list;
    list.push_back(std::make_unique<Data>(1));
    list.push_back(std::make_unique<Data>(2));
    // スコープ終了時に自動で破棄される
}
