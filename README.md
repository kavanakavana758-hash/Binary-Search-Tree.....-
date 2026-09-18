# Binary-Search-Tree.....-
#include <iostream>
using namespace std;

template <typename T>
class BinarySearchTree
{
private:
    // BST Node
    struct Node
    {
        T data;
        Node* left;
        Node* right;

        Node(const T& value)
        {
            data = value;
            left = nullptr;
            right = nullptr;
        }
    };

    Node* root;

    // Insert helper
    Node* insert(Node* node, const T& value)
    {
        if (node == nullptr)
            return new Node(value);

        if (value < node->data)
            node->left = insert(node->left, value);
        else if (value > node->data)
            node->right = insert(node->right, value);
        else
            cout << "Duplicate value " << value << " not inserted.\n";

        return node;
    }

    // Search helper
    bool search(Node* node, const T& value) const
    {
        if (node == nullptr)
            return false;

        if (value == node->data)
            return true;

        if (value < node->data)
            return search(node->left, value);

        return search(node->right, value);
    }

    // Find minimum node
    Node* findMin(Node* node)
    {
        while (node != nullptr && node->left != nullptr)
            node = node->left;

        return node;
    }

    // Delete helper
    Node* remove(Node* node, const T& value)
    {
        if (node == nullptr)
            return nullptr;

        if (value < node->data)
        {
            node->left = remove(node->left, value);
        }
        else if (value > node->data)
        {
            node->right = remove(node->right, value);
        }
        else
        {
            // Case 1: No child
            if (node->left == nullptr && node->right == nullptr)
            {
                delete node;
                return nullptr;
            }

            // Case 2: Only right child
            if (node->left == nullptr)
            {
                Node* temp = node->right;
                delete node;
                return temp;
            }

            // Case 3: Only left child
            if (node->right == nullptr)
            {
                Node* temp = node->left;
                delete node;
                return temp;
            }

            // Case 4: Two children
            Node* temp = findMin(node->right);
            node->data = temp->data;
            node->right = remove(node->right, temp->data);
        }

        return node;
    }

    // In-order traversal
    void inOrder(Node* node) const
    {
        if (node == nullptr)
            return;

        inOrder(node->left);
        cout << node->data << " ";
        inOrder(node->right);
    }

    // Pre-order traversal
    void preOrder(Node* node) const
    {
        if (node == nullptr)
            return;

        cout << node->data << " ";
        preOrder(node->left);
        preOrder(node->right);
    }

    // Post-order traversal
    void postOrder(Node* node) const
    {
        if (node == nullptr)
            return;

        postOrder(node->left);
        postOrder(node->right);