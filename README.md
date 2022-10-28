# ACC-45-DAYS-OF-CODE

 # DAY 1
QUESTION:
# Given a string s, return the longest palindromic substring in s.

# A string is called a palindrome string if the reverse of that string is the same as the original string

SOLUTION:

class Solution
{
public:
    bool isPalindrome(string s)
    {
        int i = 0, j = s.size() - 1;

        while (i < j)
        {
            if (s[i++] != s[j--])
                return false;
        }
        return true;
    }

    string longestPalindrome(string s)
    {
        int n = s.size();
        if (n == 0)
            return "";

        if (n == 1)
            return s;

        string result = "";
        for (int i = 0; i < n - 1; i++)
        {
            for (int j = 1; j <= n - i; j++)
            {
                if (isPalindrome(s.substr(i, j)))
                {
                    if (result.size() < j)
                        result = s.substr(i, j);
                }
            }
        }
        return result;
    }
};

# DAY 2


QUESTION:
# Given an input string s and a pattern p, implement regular expression matching with support for '.' and '*' where:

# '.' Matches any single character.​​​​
# '*' Matches zero or more of the preceding element.
# The matching should cover the entire input string (not partial).

SOLUTION:
class Solution {
public:
    int dp[21][31];
    bool func(int i,int j,string &s,string &p){
        if(dp[i][j] != -1) return dp[i][j];
        if(i >= s.size()){
            if(j == p.size()) return dp[i][j] = true;
            if(j + 1 < p.size() && p[j + 1] == '*') return dp[i][j] = func(i,j + 2,s,p);
            return dp[i][j] = false;
        }
        else if(j == p.size()) return dp[i][j] = false;
        bool match = (s[i] == p[j] || p[j] == '.');
        bool ans = false;
        if(match){
            if(j + 1 < p.size() && p[j + 1] == '*') ans = func(i+1,j,s,p);
            if(!ans) ans = func(i + 1,j + 1,s,p);
        }
        if(!ans && j + 1 < p.size() && p[j + 1] == '*') ans = func(i,j + 2,s,p);
        return dp[i][j] = ans;
    }
    bool isMatch(string s, string p) {
        memset(dp,-1,sizeof dp);
        return func(0,0,s,p);
    }
};

# DAY 3
QUESTION:
# Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.
SOLUTION:
class Solution {
public:
    
void solve(vector<int>&nums,int ind,vector<vector<int>>&ans,int max)
{
	if(ind >= max)
	{
		ans.push_back(nums);
		return ;
	}
	for(int i=ind;i<max;i++)
	{
		swap(nums[ind],nums[i]);
		solve(nums,ind+1,ans,max);
		swap(nums[i],nums[ind]);
	}
	
}

 vector<vector<int>> permute(vector<int>& nums) {
	
	vector<vector<int>> ans;        
	
	int max = nums.size();
	
	solve(nums,0,ans,max);
	
	return ans;

}

};

# DAY 4
QUESTION:
# Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

SOLUTION:
	class Solution {
public:

//global values shared between both function
vector<vector<int>>result;
vector<int>current;
int sum;

void function(vector<int>& candidates,int target,int index)
{
    if(sum>target)return ;//base case if sum is greater then target then return 
    
    if(sum==target){
        result.push_back(current);//id sum is equal to target then just add current to result
    }
    for(int i=index;i<candidates.size();i++){
        sum+=candidates[i];//and current value to sum
        current.push_back(candidates[i]);//and current value to current vector
        function(candidates,target,i);//again reccure for same index i
        sum-=candidates[i];//back track mean remove value that previously added
        current.pop_back();//remove the value that previously added to current 
    }      
}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    sum=0; // This sum is global you can see it on the top of code just a clarify if you have any doubt
    function(candidates,target,0);//i make result current and sum global to reduce the size of function for simplecity
    return result;//This result is also global 
}
};
'''
Now second approch is bit tricky you have to be bit carefull or you can solve this approch using pen and paper

'''
class Solution {
public:

vector<vector<int>>result;
vector<int>current;

void function(vector<int>& candidates,int target,int index)
{
    if(target==0){
        result.push_back(current);
        return;
    }
    
    if(index==candidates.size() || target<0)return;
    
    current.push_back(candidates[index]);
    function(candidates,target-candidates[index],index);
    current.pop_back();
    function(candidates,target,index+1);      
}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    function(candidates,target,0);
    return result;
}
	
# DAY 5
QUESTION:
# Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

SOLUTION:
	class Solution {
public:
  vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>>ans;
        int n=strs.size();
        map<string,vector<string>>mp;
        for(int i=0;i<n;i++){
            string temp=strs[i];
            sort(strs[i].begin(),strs[i].end());
            mp[strs[i]].push_back(temp);
        }
        for(auto it:mp){
            ans.push_back(it.second);
        }
        return ans;
    }
};
 
# DAY 6
QUESTION:
# Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.
SOLUTION:
	class Solution {
public:
    void reverse(string &res)
    {
        int n=res.size();
        int i=0,j=n-1;
        while(i<j)
        {
            swap(res[i],res[j]);
            i++;j--;
        }
    }
    string helpmul(string s,int a)
    {
        int n=s.length();
        string res="";
        int c=0;
        for(int i=n-1;i>=0;i--)
        {
            int x=(s[i]-'0')*a + c;
            res+=to_string(x%10);
            c=x/10;
        }
        if(c)
            res+=to_string(c);
        
        reverse(res);
        return res;
    }
    string add(string s1,string s2)
    {
        if(s1.length()<s2.length())
            swap(s1,s2);
        
        int n=s1.length();
        int m=s2.length();
        int c=0;
        string res="";
        int i=n-1,j=m-1;
        while(i>=0)
        {
            int x=(s1[i]-'0')+(j>=0?(s2[j]-'0'):0)+c;
            res+=to_string(x%10);
            c=x/10;
            i--;j--;
        }
        if(c)
            res+=to_string(c);
        
        reverse(res);
        return res;
    }
    
    string multiply(string num1, string num2) {
        int n=num1.length(),m=num2.length();
        string zero="";
        if(num1=="0" || num2=="0")
            return "0";
        string ans="";
        for(int i=m-1;i>=0;i--)
        {
            int a=num2[i]-'0';
            string curr=helpmul(num1,a);
            curr+=zero;
            zero+='0';
            ans=add(ans,curr);
        }
        return ans;
    }
};		      
	
	
# DAY 7
QUESTION:
# Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

SOLUTION:
	class Solution {
public:

vector<int> searchRange(vector<int>& nums, int tg) {
    int x1=lower_bound(nums.begin(),nums.end(),tg)-nums.begin();
    int x2=upper_bound(nums.begin(),nums.end(),tg)-nums.begin()-1;
    if(binary_search(nums.begin(),nums.end(),tg))
    {
        return {x1,x2};
    }
    return {-1,-1};
}
};
 
# DAY 8
QUESTION:
	Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

Each of the digits 1-9 must occur exactly once in each row.
Each of the digits 1-9 must occur exactly once in each column.
Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
The '.' character indicates empty cells.
SOLUTION:
bool isValid(vector<vector<char>>& board, int row, int col, char c) {
    // row check
    for(int i = 0; i < 9; i++) 
		if(board[i][col] == c) 
			return false;
	// col check
    for(int i = 0; i < 9; i++) 
		if(board[row][i] == c) 
			return false;
    // box check
    int x0 = (row/3) * 3, y0 = (col/3) * 3;
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            if(board[x0 + i][y0 + j] == c) return false;
        }
    }
    return true;
}

bool helper(vector<vector<char>> &board, int row, int col) {
    // done
    if(row == 9) 
		return true;
    // time for next row
    if(col == 9) 
		return helper(board, row + 1, 0);
    // already marked
    if(board[row][col] != '.') 
		return helper(board, row, col + 1);

    for(char c = '1'; c <= '9'; c++) {
        if(isValid(board, row, col, c)) {
            board[row][col] = c;
            // without return here, the board reverts to initial state
            if(helper(board, row, col + 1))
				return true;
            board[row][col] = '.';
        }
    }
    return false;
}

void solveSudoku(vector<vector<char>>& board) {
    helper(board, 0, 0);
}
