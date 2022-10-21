---
title: 서울과기대 알고리즘 대회 출제 후기
tags: [ps]
style: fill
color: primary
date: 2022-10-21 00:01:00+0900
description: 이번이 세 번째 대회 출제입니다
external_url:
---
# 어쩌다, 대회 출제자
처음 시작은 2년전 2020년 경북대학교 알고리즘 대회(Goricon 2020) 였다. 당시 알고리즘 동아리 Gori의 회장으로 있던 나는 19년도에 동아리 회원으로서 Goricon에 참여했던 것과는 달리 출제자의 입장에 서보았고 그것은 나에게 ps문제를 푸는 것 뿐만 아니라 만드는 것도 재밌음을 깨닫게 해준 좋은 기회였다. polygon이라는 사이트가 뭔지도 몰라서 한참 구글에도 뒤져보고 여러가지 알아보느라 힘들었던 게 2년이나 지났다는 점은 잘 와닫지 않는다. 조만간 알고리즘 문제 제작을 시작하는 사람들을 위해 polygon 소개글을 써보려고 생각중이다.

# 제1회 서울과학기술대학교 컴퓨터공학과 알고리즘 대회
백준에서 우연히 출제자를 모집한다는 글을 보게 되었다. 마침 예전부터 생각해둔 문제가 있었기에 망설임없이 출제를 하기로 결심했다.

## 유전자 조합
[백준 25758](https://www.acmicpc.net/problem/25758)

문제에서 주어진대로 모든 조합을 살펴보기에는 O(N^2)의 시간이 걸린다. 하지만 문제의 입력이 대문자 알파벳 두 글자라는 점을 이용하면 비둘기집 원리에 의해 입력 크기를 26*26으로 줄일 수 있다. 그렇게 한 다음에는 주어진대로 구현을 하면 되는 문제였다.

<details>
<summry>정해</summary>
<div markdown="1">

```
#include <bits/stdc++.h>
using namespace std;

vector<string> arr;
int n;
bool chk[26];
int cnt[1000];

int str_to_int(string& s)
{
    return (s[0]-'A') + (s[1]-'A')*26;
}

string int_to_str(int i)
{
    string s;
    s.push_back(i%26+'A');
    s.push_back(i/26+'A');
    return s;
}

int main()
{
	cin>>n;
	arr.resize(n);
	for(int i=0;i<n;++i)
	{
		cin>>arr[i];
		cnt[str_to_int(arr[i])]++;
	}
	for(int i=0;i<1000;++i)
	{
	    if(cnt[i] > 1)
	    {
	        string s = int_to_str(i);
	        chk[max(s[0], s[1])-'A'] = true;
	    }
	}
	sort(arr.begin(),arr.end());
	arr.erase(unique(arr.begin(),arr.end()), arr.end());
	for(int i=0;i<arr.size();++i)
	{
		for(int j=i+1;j<arr.size();++j)
		{
			chk[max(arr[i][0],arr[j][1])-'A'] = true;
			chk[max(arr[j][0],arr[i][1])-'A'] = true;
		}
	}
	vector<char> ret;
	for(int i=0;i<26;++i)
	{
		if(chk[i])
			ret.push_back(i+'A');
	}
	cout<<ret.size()<<'\n';
	for(auto i : ret)
	{
		cout<<i<<' ';
	}
	return 0;
}

```
</div>
</details>

## 들판 건너가기
[백준 25759](https://www.acmicpc.net/problem/25759)

LIS문제를 풀어봤다면 익숙한 dp식이 생각날 것이다.

dp[i] = k: (1 -> i-1) max(dp[k]+pow(arr[i]-arr[k], 2))

이 식을 계산하는 데에는 O(N^2)의 시간이 필요하다. 하지만 입력으로 주어지는 숫자가 100이하라는 것을 이용해 1~100까지의 값만 관리하면 100*N으로 줄일 수 있다. 또 다른 풀이로는 CHT를 사용한 풀이가 있다. 검수 중에 발견됬는데 나도 CHT는 자세하게 공부하진 않아서 보고 조금 놀랐다.

<details>
<summry>정해</summary>
<div markdown="1">

```
#include <bits/stdc++.h>
using namespace std;
#define int ll
typedef long long ll;

int dp[100100];
int num_max[110];
int arr[100100];

int32_t main() {
	int n;
	cin>>n;
	memset(num_max, -1, sizeof(num_max));
	for(int i=0;i<n;++i)
	{
		cin>>arr[i];
	}
	for(int i=0;i<n;++i)
	{
		for(int j=1;j<=100;++j)
		{
			if(num_max[j] == -1)
				continue;
			dp[i] = max(dp[i], num_max[j]+(arr[i]-j)*(arr[i]-j));
		}
		num_max[arr[i]] = max(num_max[arr[i]], dp[i]);
	}
	cout<<dp[n-1];
	return 0;
}
```
</div>
</details>

# 결론
다음 대회 출제는 Goricon 2022로 예정되어 있다. 이번에도 별 문제없이 잘 할 수 있었으면 좋겠다.