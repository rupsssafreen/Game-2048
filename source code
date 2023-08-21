#include <bits/stdc++.h>
using namespace std;

/////////////////////////////////////////////////////////////////////////////////////////////////
///////////// PRE-DEFINED FUNCTIONS SECTION /////////////////////////////////////////////////////
///////////// DO NOT CHANGE ANYTHING, OTHERWISE YOU MIGHT GET WRONG ANSWER VERDICT //////////////
/////////////////////////////////////////////////////////////////////////////////////////////////

/*
    Parameters and seed values for the random-number generator
*/
const int M = (1 << 16) | 1;
const int A = 75;
const int C = 74;
int Xn = 0;

/*
    Return: returns a random (pseudo-random) number
*/
int generateRandomNumber()
{
    Xn = ((A * Xn) + C) % M;
    return Xn;
}

/*
    Arguments: take in a list of (x, y) coordinates of all the empty cells
    Return: returns a value (80% : 2, 20% : 4) along with a Randomly (Pseudo)
    selected cell out of the empty cells
    pair<int, pair<int, int>> --> <value, <x, y>>
*/

pair<int, pair<int, int>> getRandomEmptyCellAndValue(vector<pair<int, int>> &emptyCells)
{
    int mini = 0, maxi = (int)emptyCells.size() - 1;
    int pos = (generateRandomNumber() % (maxi - mini + 1)) + mini;

    sort(emptyCells.begin(), emptyCells.end());

    mini = 1, maxi = 10;
    int value = (generateRandomNumber() % (maxi - mini + 1)) + mini;

    if (value <= 8)
        return make_pair(2, emptyCells[pos]);

    return make_pair(4, emptyCells[pos]);
}

/////////////////////////////////////////////////////////////////////////////////////////////////
///////////// COMPLETE FOLLOWING FUNCTIONS //////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////////////

int board[4][4];
int numEmptyCells;

/*
    Print the current configuration of the board
*/
void printBoard()
{
    for (int i = 0; i < 4; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            if (board[i][j] == 0)
                cout << "-";
            else
                cout << board[i][j];
            cout << "\t";
        }
        cout << "\n";
    }
}

/*
    Auxiliary / Helper Functions for move():
    1. gravity()
    2. merge()
    3. pushToLeft():
*/

/*
    Arguments: takes in the vector of values as reference
    Push all the elements to the left without leaving empty spaces
    Changes the vector in-place
*/
void gravity(vector<int> &arr)
{
    int firstEmpty = 0;
    for (int i = 0; i < 4; i++)
    {
        if (arr[i] != 0)
        {
            swap(arr[i], arr[firstEmpty]);
            firstEmpty++;
        }
    }
}

/*
    Arguments: takes in the vector of values as reference
    Merge the consecutive elements
    Changes the vector in-place
*/
void merge(vector<int> &arr)
{
    for (int i = 1; i < 4; i++)
    {
        if (arr[i] == arr[i - 1] && arr[i] != 0)
        {
            arr[i - 1] = 2 * arr[i - 1];
            arr[i] = 0;
            numEmptyCells++;
        }
    }
}

/*
    Arguments: takes in the vector of values as reference
    Return: returns true if the move changed the array, else false
*/
bool pushToLeft(vector<int> &arr)
{
    vector<int> copyArr(arr);
    gravity(arr);
    merge(arr);
    gravity(arr);
    for (int i = 0; i < 4; i++)
    {
        if (copyArr[i] != arr[i])
            return true;
    }
    return false;
}

/*
    Arguments: takes in the direction of the move {1: left, 2: down, 3: right, 4: up}
    Return: after applying the move to the board, returns if the move was successful in changing the board or not
*/
bool move(int direction)
{
    bool change = false;
    vector<int> curr;

    // Left
    if (direction == 1)
    {
        for (int i = 0; i < 4; i++)
        {

            // copy the values in a particular direction
            for (int j = 0; j < 4; j++)
                curr.push_back(board[i][j]);

            // push to left and check if anything has changed
            change |= pushToLeft(curr);

            // copy the changed values back to original
            for (int j = 3; j >= 0; j--)
            {
                board[i][j] = curr.back();
                curr.pop_back();
            }
        }
    }

    // Down
    else if (direction == 2)
    {
        for (int j = 0; j < 4; j++)
        {

            // copy the values in a particular direction
            for (int i = 3; i >= 0; i--)
                curr.push_back(board[i][j]);

            // push to left and check if anything has changed
            change |= pushToLeft(curr);

            // copy the changed values back to original
            for (int i = 0; i < 4; i++)
            {
                board[i][j] = curr.back();
                curr.pop_back();
            }
        }
    }

    // Right
    else if (direction == 3)
    {
        for (int i = 0; i < 4; i++)
        {

            // copy the values in a particular direction
            for (int j = 3; j >= 0; j--)
                curr.push_back(board[i][j]);

            // push to left and check if anything has changed
            change |= pushToLeft(curr);

            // copy the changed values back to original
            for (int j = 0; j < 4; j++)
            {
                board[i][j] = curr.back();
                curr.pop_back();
            }
        }
    }

    // Up
    else
    {
        for (int j = 0; j < 4; j++)
        {

            // copy the values in a particular direction
            for (int i = 0; i < 4; i++)
                curr.push_back(board[i][j]);

            // push to left and check if anything has changed
            change |= pushToLeft(curr);

            // copy the changed values back to original
            for (int i = 3; i >= 0; i--)
            {
                board[i][j] = curr.back();
                curr.pop_back();
            }
        }
    }
    return change;
}

/*
    Randomly selects a cell (using getRandomEmptyCellAndValue() function) 
    and assigns a value to it
*/
void populateRandomCell()
{
    vector<pair<int, int>> emptyCells;
    for (int i = 0; i < 4; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            if (board[i][j] == 0)
                emptyCells.push_back({i, j});
        }
    }
    auto valAndCoord = getRandomEmptyCellAndValue(emptyCells);
    int val = valAndCoord.first;
    int x = valAndCoord.second.first, y = valAndCoord.second.second;
    board[x][y] = val;
    numEmptyCells--;
}

/*
    Return: returns the current status of the game
    {
        0: Game Over (You lose / No more possible moves left)
        1: Game Over (You win / Target Achieved)
        2: Game still in progress (Valid moves left)
    }
*/
int checkGameStatus()
{
    bool moveAvailable = false;
    for (int i = 0; i < 4; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            if (board[i][j] == 2048)
                return 1;
            if (i < 3 && board[i][j] == board[i + 1][j])
                moveAvailable = true;
            if (j < 3 && board[i][j] == board[i][j + 1])
                moveAvailable = true;
        }
    }
    if (numEmptyCells == 0 && (!moveAvailable))
        return 0;
    return 2;
}

/*
    Used to Initialize the Board and other values required by the game
*/
void initialize()
{
    memset(board, 0, sizeof(board));
    numEmptyCells = 16;
    populateRandomCell();
    populateRandomCell();
}

/*
    Runner to run the game
*/
struct Runner
{

    string boardNotChangedMessage = "Invalid Move";
    string gameWonMessage = "Game Over, You Win";
    string gameLostMessage = "Game Over, You Lose";

    Runner(){};

    void play()
    {
        int n; cin >> n;
        initialize();
        printBoard();

        for(int i=0; i<n; i++)
        {
            int moveDir;
            cin >> moveDir;
            bool validMove = move(moveDir);

            if (!validMove)
            {
                cout << boardNotChangedMessage << "\n";
                continue;
            }

            populateRandomCell();
            printBoard();

            int gameStatus = checkGameStatus();

            if (gameStatus == 0)
            {
                cout << gameLostMessage << "\n";
                return;
            }
            else if (gameStatus == 1)
            {
                cout << gameWonMessage << "\n";
                return;
            }
        }
    }
};

int main()
{

    Runner game;
    game.play();

    return 0;
}
