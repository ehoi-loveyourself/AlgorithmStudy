# 📘 1031 (박태이)

## 소요시간, 메모리
- 62960 KB
- 808 ms

## 풀이 방법
- 다익스트라와 플로이드 워셜에 대해 다시 공부했습니다.
- 해당 문제와 같은 경우에는 한 시작점에서 다른 특정 지점까의 최단 경로/길이를 계산하는 것이 아닌 모든 지점에서 모든 지점까지의 최단 길이를 알아야 하는 문제였기에 플로이드 워셜로 풀어야 한다는 것을 알았습니다.
- 여기서 핵심 논리는 i에서 j로 갈때 k를 거쳐 가는 것과 바로 가는 것 중 최솟값으로 갱신하는 것이라고 생각합니다. (이 과정에서 3중 포문이 돌아서 시간 복잡도가 N^3)
- 풀이 방법은 간략히 코드와 함께 주석으로 작성하였습니다.

## Code
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class DAY221031_BOJ1956_G4_운동 {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        // 1. 입력
        int V = Integer.parseInt(st.nextToken()); // 마을
        int E = Integer.parseInt(st.nextToken()); // 도로

        int[][] map = new int[V + 1][V + 1];

        int INF = 987654321;

        // 일단 자기자신으로 가는 거 빼고 무한으로 설정
        for (int i = 1; i <= V; i++) {
            for (int j = 1; j <= V; j++) {
                if (i == j) continue;
                map[i][j] = INF;
            }
        }

        // 이제 입력 받기
        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());

            map[a][b] = c;
        }

        // 2. 이제 모든 지점에서 다른 모든 지점까지의 최단 경로를 구해야 한다
        // 논리 : [i][j] = min ([i][j], [i][k] + [k][j] 로 최단 경로 갱신
        for (int k = 1; k <= V; k++) {
            for (int i = 1; i <= V; i++) {
                for (int j = 1; j <= V; j++) {
                    map[i][j] = Math.min(map[i][j], map[i][k] + map[k][j]);
                }
            }
        }

        // 3. 이제 문제에서 요구하는 답을 구하기
        // 사이클을 이루는 도로 중 길이의 합이 최소인 것
        int ans = INF;

        // 사이클이 있다는 것 : [i][j]가 무한이 아니고 [j][i]가 무한이 아니면 됨
        for (int i = 1; i <= V; i++) {
            for (int j = 1; j <= V; j++) {
                if (i == j) continue; // 얘는 무조건 0이니까

                if (map[i][j] != INF && map[j][i] != INF) {
                    ans = Math.min(ans, map[i][j] + map[j][i]);
                }
            }
        }

        // 4. 출력
        // 실수 : 사이클이 없을 때는 -1을 출력하라고 한 것을 놓쳤다.
        ans = (ans == INF) ? -1 : ans;
        System.out.println(ans);
    }
}

```
