#include <iostream>
#include <queue>
#include <unordered_map>
using namespace std;
struct Node {
    char ch;
    int freq;
    Node *left, *right;
};
Node* createNode(char ch, int freq, Node* left = nullptr, Node* right = nullptr) {
    Node* node = new Node;
    node->ch = ch;
    node->freq = freq;
    node->left = left;
    node->right = right;
    return node;
}
struct compare {
    bool operator()(Node* l, Node* r) {
        return l->freq > r->freq;
    }
};
Node* buildHuffmanTree(const unordered_map<char, int>& freqMap) {
    priority_queue<Node*, vector<Node*>, compare> pq;
    for (auto& entry : freqMap) {
        pq.push(createNode(entry.first, entry.second));
    }
    while (pq.size() != 1) {
        Node* left = pq.top();
        pq.pop();
        Node* right = pq.top();
        pq.pop();
        Node* merged = createNode('$', left->freq + right->freq, left, right);
        pq.push(merged);
    }
    return pq.top();
}
void printCodes(Node* root, string code = "") {
    if (!root)
        return;
    if (root->ch != '$') {
        cout << root->ch << ": " << code << endl;
    }
    printCodes(root->left, code + "0");
    printCodes(root->right, code + "1");
}
int main() {
    string input;
    cout << "nhap ten: ";
    getline(cin, input);
    unordered_map<char, int> freqMap;
    for (char ch : input) {
        freqMap[ch]++;
    }
    Node* root = buildHuffmanTree(freqMap);
    cout << "Huffman code:" << endl;
    printCodes(root);

    return 0;
}
