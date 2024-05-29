#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <iomanip>
#include <cstdlib>
#include <ctime>
#include <algorithm>
#include <random>

using namespace std;

const int ROW = 21;
const int COL = 21;

vector<vector<int>> maze(ROW, vector<int>(COL, 1)); // Initialize all cells as walls

struct Point {
    int x, y;
};

void printMaze(const vector<Point>& path = {}, const string& algorithm = "") {
    // Function to print the maze with optional path and algorithm name
    vector<vector<char>> displayMaze(ROW, vector<char>(COL, ' '));
    // Convert maze to a printable format
    for (int i = 0; i < ROW; ++i) {
        for (int j = 0; j < COL; ++j) {
            displayMaze[i][j] = (maze[i][j] == 1) ? '#' : '.';
        }
    }
    // Mark the path with '*'
    for (const auto& p : path) {
        displayMaze[p.x][p.y] = '*';
    }
    // Print the maze with optional algorithm name
    if (!algorithm.empty()) {
        cout << algorithm << " found a path:" << endl;
    } else {
        cout << "Generated maze:" << endl;
    }
    // Print the maze grid
    cout << "+";
    for (int i = 0; i < COL; ++i) cout << "--";
    cout << "+" << endl;
    for (int i = 0; i < ROW; ++i) {
        cout << "|";
        for (int j = 0; j < COL; ++j) {
            cout << " " << displayMaze[i][j];
        }
        cout << " |" << endl;
    }
    cout << "+";
    for (int i = 0; i < COL; ++i) cout << "--";
    cout << "+" << endl;
    cout << endl;
}

bool isValid(int x, int y) {
    // Check if a cell is valid (within maze boundaries and not a wall)
    return x >= 0 && x < ROW && y >= 0 && y < COL && maze[x][y] == 0;
}

void BFS(Point start, Point end) {
    // Breadth-first search algorithm
    vector<vector<bool>> visited(ROW, vector<bool>(COL, false));
    queue<pair<Point, vector<Point>>> q;
    q.push({start, {start}});
    visited[start.x][start.y] = true;
    int rowNum[] = {-1, 0, 0, 1};
    int colNum[] = {0, -1, 1, 0};
    while (!q.empty()) {
        auto current = q.front();
        Point pt = current.first;
        vector<Point> path = current.second;
        q.pop();
        if (pt.x == end.x && pt.y == end.y) {
            printMaze(path, "BFS");
            return;
        }
        for (int i = 0; i < 4; i++) {
            int row = pt.x + rowNum[i];
            int col = pt.y + colNum[i];
            if (isValid(row, col) && !visited[row][col]) {
                visited[row][col] = true;
                path.push_back({row, col});
                q.push({{row, col}, path});
                path.pop_back();
            }
        }
    }
    cout << "No path found by BFS" << endl;
}

void DFSUtil(Point pt, Point end, vector<Point>& path, vector<vector<bool>>& visited, bool& found) {
    // Utility function for depth-first search algorithm
    if (pt.x == end.x && pt.y == end.y) {
        found = true;
        printMaze(path, "DFS");
        return;
    }
    int rowNum[] = {-1, 0, 0, 1};
    int colNum[] = {0, -1, 1, 0};
    for (int i = 0; i < 4; i++) {
        int row = pt.x + rowNum[i];
        int col = pt.y + colNum[i];
        if (isValid(row, col) && !visited[row][col]) {
            visited[row][col] = true;
            path.push_back({row, col});
            DFSUtil({row, col}, end, path, visited, found);
            if (found) return;
            path.pop_back();
            visited[row][col] = false;
        }
    }
}

void DFS(Point start, Point end) {
    // Depth-first search algorithm
    vector<vector<bool>> visited(ROW, vector<bool>(COL, false));
    vector<Point> path;
    bool found = false;
    visited[start.x][start.y] = true;
    path.push_back(start);
    DFSUtil(start, end, path, visited, found);
    if (!found) {
        cout << "No path found by DFS" << endl;
    }
}

void generateMaze() {
    // Function to generate a random maze
    random_device rd; // Non-deterministic generator
    mt19937 g(rd()); // Standard mersenne_twister_engine seeded with rd()
    stack<Point> s;
    Point start = {1, 1};
    maze = vector<vector<int>>(ROW, vector<int>(COL, 1)); // Reset maze
    maze[start.x][start.y] = 0;
    s.push(start);
    int rowNum[] = {-2, 0, 0, 2};
    int colNum[] = {0, -2, 2, 0};
    while (!s.empty()) {
        Point current = s.top();
        vector<int> directions = {0, 1, 2, 3};
        shuffle(directions.begin(), directions.end(), g);
        bool moved = false;
        for (int i = 0; i < 4; ++i) {
            int dir = directions[i];
            int newRow = current.x + rowNum[dir];
            int newCol = current.y + colNum[dir];
            if (newRow > 0 && newRow < ROW - 1 && newCol > 0 && newCol < COL - 1 && maze[newRow][newCol] == 1) {
                maze[newRow][newCol] = 0;
                maze[current.x + rowNum[dir]/2][current.y + colNum[dir]/2] = 0;
                s.push({newRow, newCol});
                moved = true;
                break;
            }
        }
        if (!moved) {
            s.pop();
        }
    }
}

int main() {
    generateMaze(); // Generate maze
    printMaze(); // Print the generated maze
    Point start = {1, 1};
    Point end = {ROW - 2, COL - 2};
    cout << "Maze Runner using BFS and DFS algorithms\n";
    cout << "# represents walls, . represents paths, and * represents the path found.\n" << endl;
    BFS(start, end);
    DFS(start, end);
