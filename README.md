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
# Write a program to solve a Sudoku puzzle by filling the empty cells.

# A sudoku solution must satisfy all of the following rules:

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

# DAY 9
QUESTION:
# You are given two jugs with capacities jug1Capacity and jug2Capacity liters. There is an infinite amount of water supply available. Determine whether it is possible to measure exactly targetCapacity liters using these two jugs.

If targetCapacity liters of water are measurable, you must have targetCapacity liters of water contained within one or both buckets by the end.

Operations allowed:

Fill any of the jugs with water.
Empty any of the jugs.
Pour water from one jug into another till the other jug is completely full, or the first jug itself is empty.
 
SOLUTION:
	'class Solution
{
public:
    bool canMeasureWater(int x, int y, int z)
    {
        if (x + y == z || x == z || y == z)
            return true;
        if (x + y < z)
            return false;
        int m = x + y; // max capacity
        vector<int> v = {x, -x, y, -y};
        queue<int> q;
        q.push(0);
        vector<int> vis(m + 1, 0);
        vis[0] = 1;
        while (!q.empty())
        {
            int curr = q.front();
            q.pop();
            if (curr == z)
                return true;
            for (int i = 0; i < 4; i++)
            {
                int k = curr + v[i];
                if (k >= 0 && k <= m && vis[k] == 0)
                {
                    q.push(k);
                    vis[k] = 1;
                }
            }
        }
        return false;
    }
			
};

# DAY 10	
QUESTION:
# You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
You may not modify the values in the list's nodes. Only nodes themselves may be changed.
ANSWER:
		    class Solution {
public:
    void reorderList(ListNode* head) {
        
//      step 1 : Finding the middle of the list in order to split it in 2 parts
        ListNode *slow=head;
        ListNode *fast=head->next;
        
        while(fast&&fast->next)
        {
        slow=slow->next;
        fast=fast->next->next;
        }
        
        ListNode *head2=slow->next;
        slow->next=NULL;
        
//      step 2 : Reversing the 2nd half of the list
        ListNode *forward=NULL;
        ListNode *prev=NULL;
        ListNode *curr=head2;
        
        while(curr)
        {
            forward=curr->next;
            curr->next=prev;
            prev=curr;
            curr=forward;
        }
        
//       Step 3 : Merge the 2 lists 
        head2=prev;
        while(head2)
        {
            ListNode *ptr1=head->next;
            ListNode *ptr2=head2->next;
            
            head->next=head2;
            head2->next=ptr1;
            head=ptr1;
            head2=ptr2;
            
        }
    }
};

# DAY 11
QUESTION:
#  Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
SOLUTION:
		    class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>>ans; 
        int n=strs.size();
        unordered_map<string,vector<string>>mp;
        for(auto i:strs){
            string dummy = i;
            sort(i.begin(),i.end());
            mp[i].push_back(dummy);
        }
        for(auto i:mp) ans.push_back(i.second);
        return ans;
    }
};
		    
# DAY 12
QUESTION:
# Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).
SOLUTION:
int dp[2005][2005];
class Solution {
public:
    string src , pat ;
    vector<int> yesOrNo ;
    bool solve(int i , int j){
        if(i < src.size() and j >= pat.size()) return false ;
        if(i >= src.size()) return j < pat.size() ? yesOrNo[j] : true ;
		
        if(dp[i][j] != -1) return dp[i][j] ;
        
        if(src[i] == pat[j] || pat[j] == '?') return dp[i][j] = solve(i + 1, j + 1);
        else if(pat[j] == '*') return dp[i][j] = solve(i + 1, j) or solve(i ,j + 1 ) ;
        return dp[i][j] = false ;
    }
    bool isMatch(string src, string pat) {
        for(int i = 0 ; i <= max(src.size(),pat.size()) ; ++i )
            for(int j = 0 ; j < max(src.size(),pat.size()); ++j) 
                dp[i][j] = -1 ;
        
        this->src = src  , this->pat = pat  ;
        yesOrNo.resize(pat.size(),0) ;
        
        for(int i = pat.size() - 1;  i>= 0 ; --i ){
            if(pat[i] != '*') break ;
            yesOrNo[i] = 1 ;
        }
        
        return solve(0,0) ;
    }
};
# DAY 13
QUESTION:
# Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:

Integers in each row are sorted in ascending from left to right.
Integers in each column are sorted in ascending from top to bottom.
SOLUTION:
	class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int row = matrix.size();
        int col = matrix[0].size();
        
        int rowIdx = 0;
        int colIdx = col-1;
        while(rowIdx < row && colIdx >=0){
            int e = matrix[rowIdx][colIdx];
            if(e==target){
                return true;
            }
            if(e<target){
                rowIdx++;
            }
            else{
                colIdx--;
            }
        }
        return 0;
    }	    
# DAY 14
# Given an unsorted integer array nums, return the smallest missing positive integer.

You must implement an algorithm that runs in O(n) time and uses constant extra space.
solution:
	class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int index=0,out=0,org=1;
        set<int>s;
        for(int it:nums)    s.insert(it);
        for(auto itr:s){
            if(itr>0){
                if(itr!=org)    return org;
                org++;
            }
            index++;
        }
        return org;
    }
};	    
# DAY 15
QUESTION:
# You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.
SOLUTION:
	class Solution {
public:
    bool f(int index,int n, vector<int>& nums, vector<int>&dp)
    {
        if(index ==n-1) return true;
        if(dp[index] != -1) return dp[index];
        for(int i = 1; i<=nums[index]; i++)
        {
            if(f(index+i,n,nums,dp)==true)
                return dp[index] = true;
        }
        return dp[index] = false;
    }
    bool canJump(vector<int>& nums) {
        int  n = nums.size();
        vector<int>dp(n+1,-1);
        return f(0,n,nums,dp);
       
    }
};
# DAY 16
QUESTION:
# The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

For example, for arr = [2,3,4], the median is 3.
For example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.
Implement the MedianFinder class:

MedianFinder() initializes the MedianFinder object.
void addNum(int num) adds the integer num from the data stream to the data structure.
double findMedian() returns the median of all elements so far. Answers within 10-5 of the actual answer will be accepted.
SOLUTION:
	class MedianFinder {
private:
    priority_queue<int> firstQ; // max_heap for the first half
    priority_queue<int, std::vector<int>, std::greater<int> > secQ; // min_heap for the second half
public:
    // Adds a number into the data structure.
    void addNum(int num) {
        if(firstQ.empty() || (firstQ.top()>num)) firstQ.push(num); // if it belongs to the smaller half
        else secQ.push(num); 
        
        // rebalance the two halfs to make sure the length difference is no larger than 1
        if(firstQ.size() > (secQ.size()+1))
        {
            secQ.push(firstQ.top());
            firstQ.pop();
        }
        else if(firstQ.size()+1<secQ.size())
        {
            firstQ.push(secQ.top());
            secQ.pop();
        }
    }

    // Returns the median of current data stream
    double findMedian() {
        if(firstQ.size() == secQ.size()) return firstQ.empty()?0:( (firstQ.top()+secQ.top())/2.0);
        else return (firstQ.size() > secQ.size())? firstQ.top():secQ.top(); 
    }
};
# DAY 17
QUESTION:
# Given an array rectangles where rectangles[i] = [xi, yi, ai, bi] represents an axis-aligned rectangle. The bottom-left point of the rectangle is (xi, yi) and the top-right point of it is (ai, bi).

# Return true if all the rectangles together form an exact cover of a rectangular region.
	
# SOLUTION:
	
	class Solution {
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        map<pair<int, int>, int> mark;
        for(vector<int>& x:rectangles){
            mark[{x[0], x[1]}]++;
            mark[{x[0], x[3]}]--;
            mark[{x[2], x[1]}]--;
            mark[{x[2], x[3]}]++;
        }
        int n_mark=0;
        for(auto ptr=mark.begin();ptr!=mark.end();ptr++)
            if(ptr->second!=0){
                if(abs(ptr->second)!=1) return false;
                n_mark++;
            }
        return n_mark==4;
    }
};

# DAY 18
QUESTION:
# Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
0.
# Given a roman numeral, convert it to an integer
SOLUTION:class Solution {
public:
int romanToInt(string s) {
    unordered_map<char,int> mp{
        {'I',1},
        {'V',5},
        {'X',10},
        {'L',50},
        {'C',100},
        {'D',500},
        {'M',1000},
    };
    int ans =0;
    for(int i=0;i<s.size();i++){
        if(mp[s[i]]<mp[s[i+1]])
            ans-=mp[s[i]];
        else
            ans+=mp[s[i]];
    }
    return ans;
    
}
};
	
# DAY 19
QUESTION:
# You are playing the Bulls and Cows game with your friend.

# You write down a secret number and ask your friend to guess what the number is. When your friend makes a guess, you provide a hint with the following info:

# The number of "bulls", which are digits in the guess that are in the correct position.
# The number of "cows", which are digits in the guess that are in your secret number but are located in the wrong position. Specifically, the non-bull digits in the guess that could be rearranged such that they become bulls.
# Given the secret number secret and your friend's guess guess, return the hint for your friend's guess.

The hint should be formatted as "xAyB", where x is the number of bulls and y is the number of cows. Note that both secret and guess may contain duplicate digits.
SOLUTION:
	class Solution {
public:
    string getHint(string secret, string guess) {
        vector<int> cowsS(10, 0);
        vector<int> cowsG(10, 0);

        int bulls{0}, cows{0};
        for (int i{0}; i<secret.size(); ++i) {
            // if the numbers in the positions match then it's a bull
            if (secret[i]==guess[i]) bulls++;
            else {
                // we add bulls corresponding to the numbers (eg, if secret[i]=5)
                // ascii of 5 is 53 and ascii of 0 is 48 and therefore 53-48 = 5 which is the poition given in secret[i]
                cowsS[secret[i]-'0']++;
                cowsG[guess[i]-'0']++;
            }
        }

        for (int i{0}; i<cowsG.size(); ++i) {
            // let's day cowsS[4]=5, it means that '4' is present 5 times in secret
            // same goes for cowsS. The minimum of either gives the number of matches (the extra ones can't match)
            cows+=min(cowsG[i], cowsS[i]);
        }

        // to_string converts ascii to integer. (eg., '5' to 5)
        return to_string(bulls)+'A'+to_string(cows)+'B';

    }
};

# DAY 20
QUESTION:
# You are given a 0-indexed integer array costs where costs[i] is the cost of hiring the ith worker.

# You are also given two integers k and candidates. We want to hire exactly k workers according to the following rules:

# You will run k sessions and hire exactly one worker in each session.
# In each hiring session, choose the worker with the lowest cost from either the first candidates workers or the last candidates workers. Break the tie by the smallest index.
SOLUTION:
        class Solution {
public:
    long long totalCost(vector<int>& costs, int k, int candidates) {
        // using min heap to store the cost of each candidate in the current window of size k
        priority_queue<int,vector<int>,greater<int>> pq1; 
        priority_queue<int,vector<int>,greater<int>> pq2; 
        long long ans=0; // total cost
        int l=0,r=costs.size()-1; // left and right pointer of the window
        int cnt=0; // number of candidates in the current window
        while(cnt<k){ // add the first k candidates to the window
            while(pq1.size()<candidates && l<=r) // add the first candidates to the window
                pq1.push(costs[l++]); 
            while(pq2.size()<candidates && r>=l) // add the last candidates to the window
                pq2.push(costs[r--]);
            
            int top1 = pq1.size()>0?pq1.top():INT_MAX; // if the first window is full, then the top of the first window is the minimum cost of the first candidates
            int top2 = pq2.size()>0?pq2.top():INT_MAX; // if the last window is full, then the top of the last window is the minimum cost of the last candidates
            
            if(top1<=top2){ // if the first window is cheaper, then add the first candidate to the window
                ans+=top1;
                pq1.pop(); 
            }
            else{ // if the last window is cheaper, then add the last candidate to the window
                ans+=top2;
                pq2.pop();
            }
            cnt++; // increase the number of candidates in the window
        }
        return ans; // return the total cost
    }
};

# DAY 21
QUESTION:
# Given a string paragraph and a string array of the banned words banned, return the most frequent word that is not banned. It is guaranteed there is at least one word that is not banned, and that the answer is unique.

# The words in paragraph are case-insensitive and the answer should be returned in lowercase.
SOLUTION:
	class Solution {
public:
    string mostCommonWord(string paragraph, vector<string>& banned) {
        string temp;
        vector<string> words;
        for(char c:paragraph){
            if(isalpha(c) && !isspace(c)) temp+=tolower(c);
            else{
                if(temp.length()) words.push_back(temp);
                temp="";
            }
        }
        if(temp.length()) words.push_back(temp);
        
        map<string,int> mp;
        for(string i:words) mp[i]++;
        for(string i:banned) mp[i]=0;
        string ans;
        int maxUsedFreq=0;
        for(auto i:mp){
            if(i.second>maxUsedFreq){ 
                ans=i.first;
                maxUsedFreq=i.second;
            }
        }
        return ans;
    }
};	

# DAY 22
QUESTION:
# You have a set of integers s, which originally contains all the numbers from 1 to n. Unfortunately, due to some error, one of the numbers in s got duplicated to another number in the set, which results in repetition of one number and loss of another number.

# You are given an integer array nums representing the data status of this set after the error.

# Find the number that occurs twice and the number that is missing and return them in the form of an array.
SOLUTIION:
	 ``vector<int> findErrorNums(vector<int>& nums) {
        vector<int>v;
        int t=0;//will store missing number
        int p=0;//will store repeated number 
		
         unordered_map<int,int>m;
        //storing frequency of each number in map
         for(int i=0;i<nums.size();++i)
          {
              m[nums[i]]++; 
          }
        //If frequency of any number is 2 -> store it in p
          for(auto it:m)
          {
              if(it.second==2)
              {
                  p=it.first;
              }
             
          }
        //check for missing number 
        //search for key , if key is not present store missing number in t 
           for(int i=1;i<=nums.size();++i)
           {
               auto x=m.find(i);
               if(x==m.end())
               {
                   t=i;
               }
           }
   
        v.push_back(p);
        v.push_back(t);
   
       
        return v;
    }
# DAY 23
 QUESTION:
# Given the root of a binary tree, return the inorder traversal of its nodes' values.
SOLUTION:
	
class Solution {
public:
    vector<int> ans;

    void solve(TreeNode* root) {
        if(root != NULL) {
            solve(root->left);
            ans.push_back(root->val);
            solve(root->right);
        }
    }

    vector<int> inorderTraversal(TreeNode* root) {
        solve(root);
        return ans;
    }				
# DAY 24
QUESTION:
# Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

SOLUTION:
	
	class Solution {
public:
    bool chk(TreeNode* tree1, TreeNode* tree2){
        if (tree1==NULL && tree2==NULL)return true;
        if (!tree1 && tree2)return false;
        if (!tree2 && tree1)return false;
        if (tree1->val != tree2->val)return false;
        return (chk(tree1->left,tree2->right) && chk(tree1->right,tree2->left));
    }
    bool isSymmetric(TreeNode* root) {
        if (root==NULL)return true;
        if (root->right==NULL && root->left==NULL)return true;
        if (root->left && root->right==NULL)return false;
        if (root->right && root->left==NULL)return false;
        // non null value for nodes
        return chk(root->left,root->right);
    }
};

# DAY 25
	
QUESTION:
# You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.
# Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).
# Return intervals after the insertion.
		
 
SOLUTION:
	class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
         vector<vector<int>> output;
        if(intervals.size()==0){//edge case1
            output.push_back(newInterval);
        return output;
        }
        int flag=1;
        for (int i=0;i<intervals.size();i++){
            if(intervals[i][0]>newInterval[0]){
                intervals.insert(intervals.begin() + i,newInterval);
                flag=0;
            break;
            }
        }
        if (flag==1)//edge case2
            intervals.push_back(newInterval);
        
        output.push_back(intervals[0]);
        for(int i=1;i<intervals.size();i++){
            if(output.back()[1]>=intervals[i][0])
                output.back()[1]=max(output.back()[1],intervals[i][1]);
            else
                output.push_back(intervals[i]);
        }
        return output;
    }
};

# DAY 26
	
QUESTION:
# Given an integer array nums where the elements are sorted in ascending order, convert it to a 
# height-balanced binary search tree.
	
SOLUTION:
	
	class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums, 0, nums.size()-1);
    }
    TreeNode* helper(vector<int>& nums, int low,int high){
        if(low<=high){
            int mid = low + (high-low)/2;
            TreeNode* root = new TreeNode(nums[mid]);
            root->left = helper(nums, low, mid-1);
            root->right = helper(nums, mid+1, high);
            return root;
        }
        return NULL;
    }
};

# DAY 27
QUESTION:
# You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'. The wheels can rotate freely and wrap around: for example we can turn '9' to be '0', or '0' to be '9'. Each move consists of turning one wheel one slot.
# The lock initially starts at '0000', a string representing the state of the 4 wheels.
# You are given a list of deadends dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.
# Given a target representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.
SOLUTION:
	class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        unordered_set<string> deadSet(deadends.begin(), deadends.end());
        if (deadSet.count("0000")) return -1;
        queue<string> q({"0000"});
        for (int steps = 0; !q.empty(); ++steps) {
            for (int i = q.size(); i > 0; --i) {
                auto curr = q.front(); q.pop();
                if (curr == target) return steps;
                for (auto nei : neighbors(curr)) {
                    if (deadSet.count(nei)) continue;
                    deadSet.insert(nei); // Marked as visited
                    q.push(nei);
                }
            }
        }
        return -1;
    }
    vector<string> neighbors(const string& code) {
        vector<string> result;
        for (int i = 0; i < 4; i++) {
            for (int diff = -1; diff <= 1; diff += 2) {
                string nei = code;
                nei[i] = (nei[i] - '0' + diff + 10) % 10 + '0';
                result.push_back(nei);
            }
        }
        return result;
    }
};

# DAY 28
QUESTION:
# You are given a non-negative floating point number rounded to two decimal places celsius, that denotes the temperature in Celsius.

# You should convert Celsius into Kelvin and Fahrenheit and return it as an array ans = [kelvin, fahrenheit].

# Return the array ans. Answers within 10-5 of the actual answer will be accepted.
SOLUTION:
	class Solution {
public:
    vector<double> convertTemperature(double celsius) {
        double k= celsius+273.15;         //convert celsius to kelvin using the formula : k= c+273.15
        double f= (celsius*1.80)+32.00;  //convert celsius to farenheit using the formula : f= (c*1.8)+32
        vector<double> ans= {k,f};      //store the temperature in kelvin and fahrenheit in vector
        return ans;                    // return the ans
    }
};

# DAY 29
QUESTION:
# Given a fixed-length integer array arr, duplicate each occurrence of zero, shifting the remaining elements to the right.
SOLUTION:
	class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
        int cnt=0;
        int st,end;
        for(int i=0;i<arr.size();i++){
            if(arr[i]==0){
                cnt++;
                if(cnt==arr.size()){
                    arr[arr.size()-1]=0;
                    st=i-1;
                    end=arr.size()-2;
                }
            }
            cnt++;
            if(cnt==arr.size()){
                st=i;
                end=arr.size()-1;
            }
        }
        // cout<<st<<" "<<end;
        while(st>=0){
            if(arr[st]==0){
                arr[end]=0;
                end--;
                arr[end]=0;
                end--;
                st--;
            }
            else{
                arr[end]=arr[st];
                end--;
                st--;
            }
        }
    }
};

# DAY 30
QUESTION:
# You are given an array people where people[i] is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of limit. # Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most limit.
# Return the minimum number of boats to carry every given person.

SOLUITION:
	class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
       sort(people.begin(),people.end());
       int low=0;
       int high=people.size()-1;
       int res=0;
       while(low<=high){
           if((people[low]+people[high])<=limit){
               res++;
               low++;
               high--;
           }else{
               high--;
               res++;
           }
       }
       return res;
        
                
    }
};

	
# DAY 31
QUESTION:
# Given an array of positive integers arr, return the sum of all possible odd-length subarrays of arr.

# A subarray is a contiguous subsequence of the array.
SOLUTION:
	class Solution {
public:
    int sumOddLengthSubarrays(vector<int>& arr) {
        int n = arr.size(), oddSum{0};
        for (int i=1; i<=n; i+=2) {
            int j=0;
            while (i+j<=n) {
                for (int k{0}; k<i; ++k) {
                    oddSum+=arr[k+j];
                }
                j++;
            }
        }
        return oddSum;
    }
};

# DAY 32
QUESTION:
# Given two strings s and t, return true if s is a subsequence of t, or false otherwise.

# A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).
SOLUTION:
	 				
class Solution {
public:
    bool isSubsequence(string s, string t) {
        
    int j = 0; // For index of str1 (or subsequence
 
    // Traverse str2 and str1, and
    // compare current character
    // of str2 with first unmatched char
    // of str1, if matched
    // then move ahead in str1
    for (int i = 0; i < t.length() && j < s.length(); i++)
        if (s[j] == t[i])
            j++;
 
    // If all characters of str1 were found in str2
    return (j == s.length());
    }
};
				    
# DAY 33
QUESTION:
# There is a robot starting at the position (0, 0), the origin, on a 2D plane. Given a sequence of its moves, judge if this robot ends up at (0, 0) after it completes its moves.
# You are given a string moves that represents the move sequence of the robot where moves[i] represents its ith move. Valid moves are 'R' (right), 'L' (left), 'U' (up), and 'D' (down).
# Return true if the robot returns to the origin after it finishes all of its moves, or false otherwise.						    
SOLUTION:class Solution {
public:
    bool judgeCircle(string moves) {
        int x=0;
        int y=0;
        
        for(int i=0;i<moves.size();i++){
            if(moves[i]=='U')x++;
            if(moves[i]=='D')x--;
            if(moves[i]=='R')y++;
            if(moves[i]=='L')y--;
        }
        
        return (x==0 && y==0);
    }
};

# DAY 34
QUESTION:
# Given the head of a singly linked list, return the middle node of the linked list.

# If there are two middle nodes, return the second middle node.
SOLUTION:class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* temp  = head;
        int c = 0;
        while(temp!=NULL)
        {
            c++;
            temp = temp->next;
        }
        int mid = c%2==0?c/2+1:c/2+1;
        temp = head;
        while(mid>1)
        {
            temp = temp->next;
            mid--;
        }
        return temp;
    }
};
# DAY 35
QUESTION:
# Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

# You must implement a solution with a linear runtime complexity and use only constant extra space.
SOLUTION:class Solution {
public:
    int singleNumber(vector<int>& nums) { 
       unordered_map<int,int> a;
	   for(auto x: nums)
		   a[x]++;
	   for(auto z:a)
		   if(z.second==1)
			   return z.first;
	   return -1;
    }
};
	
# DAY 36
QUESTION:
# You are given an integer n. A 0-indexed integer array nums of length n + 1 is generated in the following way:

# nums[0] = 0
# nums[1] = 1
# nums[2 * i] = nums[i] when 2 <= 2 * i <= n
# nums[2 * i + 1] = nums[i] + nums[i + 1] when 2 <= 2 * i + 1 <= n
# Return the maximum integer in the array nums​​​.
SOLUTION:
class Solution {
public:
    int getMaximumGenerated(int n) {
        vector<int> dp(n + 1, 0);
        if(n == 0)
            return 0;
        dp[1] = 1;
        int maxi = 1;
        for(int i = 2; i <= n; ++i){
            if(i & 1)
                dp[i] = dp[i >> 1] + dp[(i >> 1) + 1];
            else 
                dp[i] = dp[i >> 1];
            maxi = max(maxi, dp[i]);
        }
        return maxi;
    }
};
 
# DAY 37
# You are playing the following Nim Game with your friend:

# Initially, there is a heap of stones on the table.
# You and your friend will alternate taking turns, and you go first.
# On each turn, the person whose turn it is will remove 1 to 3 stones from the heap.
# The one who removes the last stone is the winner.
# Given n, the number of stones in the heap, return true if you can win the game assuming both you and your friend play optimally, otherwise return false.
SOLUTION:
	class Solution {
public:
    bool canWinNim(int n) {
        if(n%4!=0){
            return true;
        }
        return false;
    }
};

# DAY 38
QUESTION:
#Given an array arr of integers, check if there exist two indices i and j such that :

# i != j
# 0 <= i, j < arr.length
# arr[i] == 2 * arr[j]
SOLUTION:
	class Solution {
public:
    bool checkIfExist(vector<int>& arr) {
		
		//decalred a boolean variable ans and initialised it to false
        bool ans = false;
		
		//iterate over the vector and find two elements where either ,
		// (i) first = 2*second
		// (ii) first/2 = second
		
		/*** Don't forget to check that the one multiplied or divided with 2 , should be 
		a even number otherwise ( for 7,3  => 7/2 = 3 will return true)  ***/
		
        for(int i=0;i<arr.size();++i){		
            for(int j=0;j<arr.size();++j){
                if(i!=j && ((arr[i]%2==0 && arr[i]==2*arr[j]) || (arr[i]%2==0 && arr[i]/2==arr[j]))){
				
				//if found any such pair then assign true to the boolean variable ans and exit from the for loop
				//as there is no need to check further
				
                    ans = true;
                    break;
                }
            }
        }
         return ans;
    }
};

# DAY 39
QUESTION:
# Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).
SOLUTION:
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ones=0;
        for (int i=0; i<32; i++){
            if (n&1){
                ones++;
            }
            n= n>>1;
        }
        return ones;
    }
};

# DAY 40
QUESTION:'
# You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.

# Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return if n new flowers can be planted in the # flowerbed without violating the no-adjacent-flowers rule.
SOLUTION:class Solution {
public:
    bool canPlaceFlowers(vector<int>& fbd, int n) {
        for(int i = 0; i < fbd.size(); ++i){
            if(n == 0) break; // all plants are planted, let's stop !!!
            if((i == 0 || fbd[i-1] == 0) && fbd[i] != 1){ //checking left adj and if this place is already planted
                if(i == fbd.size()-1 || fbd[i+1] == 0){ //checking right adj
                    //now since the conditions are met we can plant here....
                    n--; // reducing the plants, since we are planting one
                    fbd[i] = 1;  // planting the plant !!
                }
            }
        }
        return n==0;
    }
};

# DAY 41
# Given two strings str1 and str2, return the shortest string that has both str1 and str2 as subsequences. If there are multiple valid strings, return any of them.

A string s is a subsequence of string t if deleting some number of characters from t (possibly 0) results in the string s.

SOLUTION:
	string shortestCommonSupersequence(string a, string b) {
    int dp[1001][1001] = {}, m = a.size(), n = b.size();
    for (int i = 0; i < m; ++i) // DP
        for (int j = 0; j < n; ++j)
            dp[i + 1][j + 1] = a[i] == b[j] ? 
                dp[i][j] + 1 : max(dp[i][j + 1], dp[i + 1][j]);
    string res;
    while (m && n) // Backtracking
        if (dp[m][n] == dp[m - 1][n])
            res += a[--m]; // Character only in a
        else if (dp[m][n] == dp[m][n - 1])
            res += b[--n]; // Character only in b
        else
            res += min(a[--m], b[--n]); // Shared character in a & b
    return a.substr(0, m) + b.substr(0, n) + string(rbegin(res), rend(res));
}
# DAY 42
QUESTION:
# You have a set of integers s, which originally contains all the numbers from 1 to n. Unfortunately, due to some error, one of the numbers in s got duplicated to another number in the set, which results in repetition of one number and loss of another number.

# You are given an integer array nums representing the data status of this set after the error.

# Find the number that occurs twice and the number that is missing and return them in the form of an array.
SOLUTION:class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        vector<int> res;
        bitset<10001> table; 
        int m = nums.size();
        int sum = (1+m)*m>>1;
        int act = 0;
        for(int i=0;i<m;i++){
            act+=nums[i];
            if(!table.test(nums[i])){
                table.set(nums[i]);
            }
            else{
                res.push_back(nums[i]);
            }
        }
        res.push_back(sum-act+res[0]);
        return res;
    }
};
 
# DAY 43
QUESTION:
# You are given a 0-indexed 2D integer array flowers, where flowers[i] = [starti, endi] means the ith flower will be in full bloom from starti to endi (inclusive). # # You are also given a 0-indexed integer array persons of size n, where persons[i] is the time that the ith person will arrive to see the flowers.
# Return an integer array answer of size n, where answer[i] is the number of flowers that are in full bloom when the ith person arrives.
SOLUTION:
	class Solution {
public:
    vector<int> fullBloomFlowers(vector<vector<int>>& flowers, vector<int>& persons) {
        map<int, int> cnt;
        int n = persons.size();
        for(auto &f : flowers) {
            cnt[f[0]]++;
            cnt[f[1] + 1]--;
        }
        cnt[1e9 + 1000] = cnt.rbegin()->second;
        vector<vector<int> > ps(persons.size());
        for(int i = 0; i < persons.size(); i++) {
            ps[i] = {persons[i], i};
        }
        sort(ps.begin(), ps.end());
        int sum = 0;
        vector<int> ans(n);
        int i = 0;
        for(auto &it : cnt) {
            int t = it.first, c = it.second;
            while(i < n && ps[i][0] < t) {
                ans[ps[i][1]] = sum;
                i++;
            }
            sum += c;
        }
        return ans;
    }
};
 
# DAY 44
QUESTION:
# We are given an array asteroids of integers representing asteroids in a row.

# For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.
# Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.
SOLUTION:
	class Solution {
public:
	vector<int> asteroidCollision(vector<int>& asteroids) {
		vector<int> res;
		if (asteroids.empty()) return res;
		bool hasCollision = false;
		res = asteroids;
		do {
			hasCollision = false;
			vector<int> tmp;
			size_t first = 0;
			while (first<res.size() && res[first]==0) ++first; /* ignore 0 */
			if (first>=res.size()) break;
			auto sz = res.size();
			tmp.push_back(res[first]);
			for (auto i=first+1; i<sz; ++i) {
				if (res[i]==0) continue; /* ignore 0 */
				if ((res[i] < 0 && tmp.back() < 0) || 
					(res[i] > 0 && tmp.back() > 0) || 
					(tmp.back() < 0 && res[i] > 0)) {
					/* no collision */
					tmp.push_back(res[i]);
				}
				else {
					if (abs(tmp.back()) > abs(res[i])) /* do nothing, the small one explode */;
					else if (abs(tmp.back()) == abs(res[i])) {
						/* both explode */
						tmp.pop_back();
						if (tmp.empty()) { /* tmp should never empty because we call tmp.back() above as default */
							i += 1;
							if (i < res.size()) tmp.push_back(res[i]);
						}
					}
					else {
						/* the small one explode */
						tmp.pop_back();
						tmp.push_back(res[i]);
					}
					hasCollision = true;
				}
			}
			res.swap(tmp);
		} while (hasCollision && res.size()>1);
		return res;
    }
};

# DAY 45
QUESTION:
# You are given an integer mass, which represents the original mass of a planet. You are further given an integer array asteroids, where asteroids[i] is the mass of the ith asteroid.
# You can arrange for the planet to collide with the asteroids in any arbitrary order. If the mass of the planet is greater than or equal to the mass of the asteroid, the asteroid is destroyed and the planet gains the mass of the asteroid. Otherwise, the planet is destroyed.
# Return true if all asteroids can be destroyed. Otherwise, return false.

 SOLUTION:
	class Solution {
public:
    bool asteroidsDestroyed(int m, vector<int>& ast) {
        
        long long int planetMass = m;
        priority_queue<int, vector<int>, greater<int>> pq;
        for(auto n : ast)
            pq.push(n);
        
        while(pq.size())
        {
            if(pq.top() <= planetMass)
            {
                planetMass+=pq.top();
                pq.pop();
            }
            else
            {
                break;
            }
        }
           
        return pq.empty();
    }
    
};	
