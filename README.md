#include <iostream>
#include <vector>
#include <string>
#include <cstdlib>
#include <cstring>
#include <unistd.h>
#include <filesystem> 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <fstream>
#include <sys/stat.h>

#define MAX_OPTIONS 100
#define MAX_PATH 256
#define MAX_ATTEMPTS 2

namespace fs = std::filesystem;

std::vector<std::string> getFiles(const std::string& directory, const std::string& extension) {
    std::vector<std::string> files;
    for (const auto& entry : fs::recursive_directory_iterator(directory)) {
        if (entry.is_regular_file() && entry.path().extension() == extension) {
            files.push_back(entry.path().string());
        }
    }
    return files;
}

using namespace std;

const string UNPACK_DIR = "/sdcard/Download/SY/放入pak";
const string OBB_DIR = "/sdcard/Download/SY/obb";

// 定义颜色常量
const std::string RESET = "\033[0m";
const std::string RED = "\033[31m";
const std::string GREEN = "\033[32m";
const std::string YELLOW = "\033[33m";
const std::string BLUE = "\033[34m";
const std::string MAGENTA = "\033[35m";
const std::string CYAN = "\033[36m";
const std::string WHITE = "\033[37m";
  
bool runCommand(const string& command) {
    return system(command.c_str()) == 0;
}


void create_dir(const string& path) {
    fs::create_directories(path);
    cout << BLUE << "目录已创建: " << path << RESET << endl;
}

void PUBG();
void banner();
void option1();
void option2();
void option3();
void jb(const std::string& selected_file);
void db(const std::string& selected_file);
void jbobb(const std::string& selected_file);
void dbobb(const std::string& selected_file);
void option8();
void wst();
void option4();
void main_menu();
void option5();
void option6();
void option7();
void option9();
void nounpackchunks(const string& selected_file);
void unpackingchunks(const string& selected_file);
void packingchunks(const string& selected_file);
void repackingpaks(const string& selected_file);
void mh();
// 提前声明新的函数
// 检查目录函数
void check_directories() {
    fs::create_directories("/sdcard/Download/SY/先进行解码解码后即可解包");
    fs::create_directories("/sdcard/Download/SY/放入map文件");
    fs::create_directories("/sdcard/Download/SY/进行打包之后加码");
}

// 执行系统命令函数
bool run_command(const std::string& command) {
    int ret = system(command.c_str());
    return ret == 0;
}

// 解码地图函数
bool decrypt_map(const std::string& input_file) {
    std::string base_name = input_file.substr(0, input_file.find_last_of('.'));
    std::string output_dir = "/sdcard/Download/SY/解码之后/" + base_name;

    fs::create_directories(output_dir);

    std::string command = "qemu-i386 $PREFIX/share/quickbms/quickbms_4gb_files script/map.bms " + input_file + " " + output_dir;
    if (run_command(command)) {
        std::cout << "decrypt map completed successfully\n";
        std::cout << "file saved at to 解码之后\n";
        return true;
    } else {
        std::cerr << RED << "Error: DECRYPT MAP FAILED!" << RESET << std::endl;
        return false;
    }
}

// 加码地图函数
bool encrypt_map(const std::string& input_file) {
    std::string base_name = input_file.substr(0, input_file.find_last_of('.'));
    std::string output_dir = "/sdcard/Download/SY/加码之后/" + base_name;

    fs::create_directories(output_dir);

    std::string command = "qemu-i386 $PREFIX/share/quickbms/quickbms_4gb_files script/map.bms " + input_file + " " + output_dir;
    if (run_command(command)) {
        std::cout << "encrypt map completed successfully\n";
        std::cout << "file saved at to 加码之后\n";
        return true;
    } else {
        std::cerr << RED << "Error: ENCRYPT MAP FAILED!" << RESET << std::endl;
        return false;
    }
}

// 获取文件列表函数
std::vector<std::string> get_files(const std::string& directory, const std::string& extension) {
    std::vector<std::string> files;
    for (const auto& entry : fs::directory_iterator(directory)) {
        if (entry.path().extension() == extension) {
            files.push_back(entry.path().string());
        }
    }
    return files;
}

// 解码菜单函数
void decmap() {
    std::string input_dir = "/sdcard/Download/SY/放入map文件";
    auto files = get_files(input_dir, ".pak");

    if (files.empty()) {
        std::cerr << RED << "No .pak files found in " << input_dir << RESET << std::endl;
        return;
    }

    std::cout << "输入选择:\n";
    for (size_t i = 0; i < files.size(); ++i) {
        std::cout << i + 1 << ". " << files[i] << "\n";
    }
    std::cout << files.size() + 1 << ". Quit\n";

    int choice;
    std::cin >> choice;

    if (choice > 0 && choice <= files.size()) {
        std::cout << "You picked " << files[choice - 1] << "\n";
        decrypt_map(files[choice - 1]);
    } else if (choice == files.size() + 1) {
        return;
    } else {
        std::cerr << "Invalid option. Try another one.\n";
    }
}
void encmap() {
    std::string input_dir = "/sdcard/Download/SY/放入map文件";
    auto files = get_files(input_dir, ".pak");

    if (files.empty()) {
        std::cerr << RED <<
