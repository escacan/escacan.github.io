---
layout: post
title: Brute-force algorithm
---

Brute-force algorithm

무식하게 푸는 방법이다. 가능한 모든 경우의 수를 구하는 것이다.( 완전 탐색 )
Ex) 두 점 사이의 최단 경로를 찾는 문제,  자원을 분배할 수 있는 경우의 수를 세는 문제

단순한 방식이지만 다른 알고리즘의 기반이 되기도 하므로 잘 익혀둘 필요가 있다.


1. 재귀호출
-	  컴퓨터가 하는 일을 작게 나누어보자. 그러면 반복되는 조각이 있다는 것을 발견할 수 있다. Ex) 완전히 같은 코드를 반복하는 for 반복문
   이러한 경우 유용하게 사용되는 개념이 재귀 함수( 재귀 호출 )이다.
   개념은 단순하다. 자신이 수행할 작업을 유사한 형태의 여러 조각으로 쪼갠 뒤 그 중 한 조각을 수행하고, 나머지를 자기 자신을 호출해 실행하는 함수이다. 

-  Sum을 재귀함수로 표현하면 어떻게 될까?

	Int recursiveSum( int n ) {
	   If ( n == 1 )  return 1; // 더 이상 쪼개지지 않을 때
	   Return n + recursiveSum( n – 1 );
	}
	
-	  한가지 유의할 점은 1부터 더해나가는 경우 재귀함수 개념을 적용할 수 없다는 점이다. 
-	  recursiveSum 에서  n이 1일 때 바로 값을 리턴한다. 더 이상 쪼개지지 않는 최소한의 작업에 도달했기에 리턴을 하도록 처리한 것이다.
   이를 “기저 사례( base case ) 라고 한다. 이때 기저사례는 모든 입력 값에 대해서 결과를 도출할 수 있어야 한다. 
   만일 n이 2일 때 리턴하게 했다면 어떨까? N이 1로 입력되었을 경우에 문제가 생긴다.

예제) 0 부터 차례대로 번호 매겨진 n개의 원소 중 네개를 고르는 모든 경우를 출력하는 코드를 작성해보라.

어떻게 접근하면 될까?  단순히 for 루프를 반복하면 된다.

for(int i= 0; i< n; i++)
for(int j= i+1; j< n; j++)
        for(int k= j+1; k< n; k++)
	       for(int l= k+1; l< n; l++)
		   cout << I << “ “ << j << “ “ << k << “ “ << l << endl;

그러나 이 방식은 골라야 할 원소의 수가 늘어나면 for 중첩문의 수가 계속 늘어난다.
또한 for 반복문은 코드가 반복된다.
	   이를 재귀함수로 표현해보자
	
	   // n: 전체 원소의 수
	   // picked: 지금까지 고른 원소들의 번호
	   // toPick: 더 고를 원소의 수
	   Void pick(int n, vector<int>& picked, int toPick) }
		// 기저 사례: 더 고를 원소가 없을 때 고른 원소들을 출력한다.
		If(toPick == 0) { printPicked(picked); return; }
		// 고를 수 있는 가장 작은 번호를 계산한다.
		Int smallest = picked.empty( ) ? 0 : picked.back( ) + 1;
		// 이 단계에서 원소 하나를 고른다.
		For( int next= smallest; next < n; ++next) {
		    Picked.push_back(next);
		    Pick(n, picked, toPick – 1);
		    Picked.pop_back( );
		}
	}

  예제: 보글 게임 ( 문제 ID : BOGGLE, 난이도 : 하 )
5x5 크기의 알파벳 격자가 있다. 상하좌우 / 대각선으로 인접한 칸들의 글자들을 이어서 단어를 찾아내는 게임이다. 
hasWord( y , x , word )  ;   게임판의 ( y, x ) 에서 시작하는 단어 word의 존재 여부를 반환한다.	
문제는 어떤 위치에서 탐색을 시작할 것인지 주어지지 않는다는 점이다. 깊게 생각하면 머리가 어지러우니 무식하게 탐색해보자.

  < 문제의 분할 > 
  게임판의 각 글자를 모두 탐색, word의 첫 글자와 다르면 false를 반환하면 된다. 만약 true라면 인접한 8개의 알파벳이 word[1] 과 같은지 탐색하면 된다.

  < 기저 사례 >
1. 위치 ( y, x )에 위치한 글자가 원하는 단어의 첫 글자가 아닌 경우
2. 1번이 아니면서 원하는 단어가 한 글자인 경우 항상 성공

여기서 팁 :  입력이 잘못되거나 범위에서 벗어난 경우도 기저 사례로 택해서 맨 처음에 처리하는 방법. 

< 구 현 >
 처음에 hasWord( x, y, word ) 를 호출했을 때, 이미 첫 글자가 일치하는지 여부는 확인했다. 이후에는 인접한 알파벳에 대해서 재귀함수를 호출하면 된다.

Const int dx[8] = {-1, -1, -1, 1, 1, 1, 0, 0};
Const int dy[8] = {-1, 0, 1, -1, 0, 1, -1, 1};

// 일치하는지 여부만 체크하면 되므로 bool 
Bool hasWord( int y, int x, const string& word) {
	// 시작위치가 범위 밖인 경우
	If( !inRange( y, x )) return false;

// 첫 글자가 일치하지 않는 경우
If( board[y][x] != word[0] ) return false;

	// 단어 길이가 1
	If( word.size( ) == 1) return true;
	
	// 인접한 알파벳 검사
	For( int direction= 0; direction < 8; direction++) {
	  Int nextY= y + dy[direction], nextX = x + dx[direction];
	  If(hasWord(nextY, nextX, word.substr(1)))
	    Return true;
	}
	Return false;
}




2. 최적화 문제
  특정 기준에 의해 가장 좋은 답을 고르는 문제이다. 적용되는 분야로는 인공지능, 생명공학, 자동차 디자인 등등 많은 것들이 있다.
 가장 유명한 문제는 외판원 문제이다( TSP ).
 외판원이 모든 도시를 방문해야 할 때, 가장 짧은 경로를 어떻게 찾아낼 수 있을까?


