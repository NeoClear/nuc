---
title: high precision
date: 2019-11-15 22:34:59
tags:
- high precision
---

为什么，这么简单的算法，我能写三个小时？？？
不过实际上细节还是比较烦的
重要的函数有plus_bit和minus_bit。不需要times_bit。
基础函数有：set_bit和get_bit。
本来打算每个位是一个uint32，后来发现此方法无法输出位10进制。。。最后采用了每个位储存10个数的做法。

```c++
#include <bits/stdc++.h>

#define BLOCK_MAX 10

typedef unsigned int uint32;
typedef unsigned long long uint64;
typedef long long int64;

using namespace std;

class integer {
public:
	bool sign = true;
	vector<uint32> dat;

	integer() {}

	integer(int val) {
		sign = val < 0 ? false : true;
		val = abs(val);
		for (int i = 0; val; i++) {
			set_bit(i, val % BLOCK_MAX);
			val /= BLOCK_MAX;
		}
	}

	integer(const integer& val): sign(val.sign), dat(val.dat.begin(), val.dat.end()) {}

	uint32 get_bit(int pos) { return pos >= dat.size() ? 0 : dat[pos]; }

	void set_bit(int pos, uint32 val) {
		while (pos >= dat.size())
			dat.push_back(0);
		dat[pos] = val;
	}

	void plus_bit(int pos, uint32 val) {
		if (val == 0)
			return;
		uint32 ans = get_bit(pos) + val;
		set_bit(pos, ans % BLOCK_MAX);
		plus_bit(pos + 1, ans / BLOCK_MAX);
	}

	void minus_bit(int pos, uint32 val) {
		if (get_bit(pos) < val)
			minus_bit(pos + 1, 1);
		set_bit(pos, get_bit(pos) < val ? get_bit(pos) + BLOCK_MAX - val : get_bit(pos) - val);
	}

	size_t len() { return dat.size(); }

	inline void clean() {
		while (!dat.empty() && dat.back() == 0)
			dat.pop_back();
		if (dat.empty())
			sign = true;
	}

	friend int abs_cmp(integer a, integer b) {
		if (a.len() != b.len())
			return a.len() < b.len() ? -1 : 1;
		for (int i = a.len() - 1; i >= 0; i--)
			if (a.dat[i] != b.dat[i])
				return a.dat[i] < b.dat[i] ? -1 : 1;
		return 0;
	}

	friend integer neg(integer e) { e.sign = !e.sign; return e; }

	friend integer operator-(integer a, integer b) {
		integer ans;
		if (a.sign != b.sign)
			return a + neg(b);
		ans.sign = abs_cmp(a, b) > 0 ? a.sign : b.sign;
		if (abs_cmp(a, b) < 0)
			swap(a, b);
		for (int i = 0; i < a.len(); i++) {
			ans.set_bit(i, a.get_bit(i));
			ans.minus_bit(i, b.get_bit(i));
		}
		ans.clean();
		return ans;
	}

	friend integer operator+(integer a, integer b) {
		integer ans;
		if (a.sign != b.sign)
			return a.sign ? a - neg(b) : b - neg(a);

		for (int i = 0; i < max(a.len(), b.len()); i++) {
			ans.set_bit(i, a.get_bit(i));
			ans.plus_bit(i, b.get_bit(i));
		}
		ans.sign = a.sign;
		ans.clean();
		return ans;
	}

	friend integer operator*(integer a, integer b) {
		integer ans;
		ans.sign = a.sign == b.sign;
		for (int i = 0; i < a.len(); i++)
			for (int j = 0; j < b.len(); j++)
				ans.plus_bit(i + j, a.dat[i] * b.dat[j]);
		ans.clean();
		return ans;
	}

	friend inline bool operator<(integer a, integer b) {
		if (a.sign != b.sign)
			return a.sign ? false : true;
		return abs_cmp(a, b) < 0 == a.sign;
	}

	friend inline bool operator==(integer a, integer b) { return a.sign == b.sign && abs_cmp(a, b) == 0; }

	friend inline bool operator>(integer a, integer b) { return b < a; }

	friend inline bool operator<=(integer a, integer b) { return !(a > b); }

	friend inline bool operator>=(integer a, integer b) { return !(a < b); }

	friend inline bool operator!=(integer a, integer b) { return !(a == b); }

	string to_string() {
		if (dat.empty())
			return "0";
		string ret;
		if (!sign)
			ret.push_back('-');
		for (auto it = dat.rbegin(); it != dat.rend(); it++)
			ret.push_back((*it) + '0');
		return ret;
	}

	friend inline ostream& operator<<(ostream& out, integer e) {
		out << e.to_string();
		return out;
	}

	friend inline istream& operator>>(istream& in, integer& e) {
		string s;
		in >> s;
		if (s.front() == '-') {
			e.sign = false;
			s = s.substr(1, s.size() - 1);
		}
		for (int i = s.size() - 1; i >= 0; i--)
			e.set_bit(s.size() - 1 - i, s[i] - '0');
		return in;
	}
};

int main() {
	integer s, b;
	cin >> s >> b;
	cout << s * b << endl;
	return 0;
}
```