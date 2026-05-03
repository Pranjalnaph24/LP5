#include <iostream>
#include <queue>
#include <vector>
#include <omp.h>
using namespace std;

int main() {

    cout << "Enter number of vertices, edges and source node:\n";
    int num_vertices, num_edges, source;
    cin >> num_vertices >> num_edges >> source;

    vector<vector<int>> adj_list(num_vertices + 1);

    cout << "Enter edges (u v):\n";
    for (int i = 0; i < num_edges; i++) {
        int u, v;
        cin >> u >> v;
        adj_list[u].push_back(v);
        adj_list[v].push_back(u);
    }

    queue<int> q;
    vector<bool> visited(num_vertices + 1, false);

    q.push(source);
    visited[source] = true;

    cout << "Parallel BFS Traversal:\n";

    while (!q.empty()) {

        int curr_vertex;

        // Safe pop
        #pragma omp critical
        {
            curr_vertex = q.front();
            q.pop();
            cout << curr_vertex << " ";
        }

        // Parallel processing of neighbors
        #pragma omp parallel for schedule(dynamic)
        for (int i = 0; i < adj_list[curr_vertex].size(); i++) {

            int neighbour = adj_list[curr_vertex][i];

            bool should_visit = false;

            // Protect visited array
            #pragma omp critical
            {
                if (!visited[neighbour]) {
                    visited[neighbour] = true;
                    should_visit = true;
                }
            }

            // Push safely
            if (should_visit) {
                #pragma omp critical
                {
                    q.push(neighbour);
                }
            }
        }
    }

    cout << endl;
    return 0;
}
