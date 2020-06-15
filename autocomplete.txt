#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include<ctype.h>
#define ALPHABET_SIZE (54)
typedef int bool;
#define true 1
#define false 0
typedef struct trie_node trie_node_t;
struct trie_node
{ char letter;
bool isWord;
trie_node_t *children[ALPHABET_SIZE]; };
typedef struct trie trie_t;
struct trie
{ trie_node_t *root;
int count; 
};
trie_node_t *makeNode(void);
void initialize(trie_t *pTrie);
void insert(trie_t *pTrie, char key[]);
int traverse(trie_t *pTrie, char key[]);
void autocomplete(trie_node_t *subTrie, char word[]);
int alphabetIndex(char c);
char indexAlphabet(int i);
trie_node_t *makeNode(void){
trie_node_t *node = NULL;
node = (trie_node_t *)malloc(sizeof(trie_node_t));
if( node ){ 
int i;
node->isWord = false;
for(i = 0; i < ALPHABET_SIZE; i++){
node->children[i] = NULL; } }
return node;}

void initialize(trie_t *pTrie)
{ pTrie->root = makeNode();

pTrie->count = 0;}
void insert(trie_t *pTrie, char key[]){
int level;
int length = strlen(key);
int index;
char x;
trie_node_t *temp;
pTrie->count++;
temp = pTrie->root;
for( level = 0; level < length; level++ ){
index = alphabetIndex(key[level]);
x = key[level];
if( !temp->children[index] ){ 
temp->children[index] = makeNode(); 
temp->children[index]->letter = x; 
}
temp = temp->children[index]; 
}
temp->isWord = true;
}
int traverse(trie_t *pTrie, char key[]){
int level;
int length = strlen(key);
int index;
trie_node_t *temp;
temp = pTrie->root;
printf("TRAVERSING TRIE FOR %s\n",key);
for( level = 0; level < length; level++ ){
index = alphabetIndex(key[level]);
if( !temp->children[index] ){
printf("WORD NOT FOUND!\n");
return 0; }
temp = temp->children[index]; }
printf("---WORD FOUND: AUTCOMPLETE---\n");
autocomplete(temp,key);
printf("---END AUTOCOMPLETE---\n");
return (0 != temp && temp->isWord); 
}
void autocomplete(trie_node_t *subTrie, char word[]){

if (subTrie != NULL){
int i;
if (subTrie->isWord == true){
printf("%s\n",word);
}
for (i = 0; i < ALPHABET_SIZE; i++){
if (subTrie->children[i] != NULL){
char *append; //Create a new string.
append = (char *)malloc(strlen(word)+1);
strcpy(append,word);
append[strlen(word)] = subTrie->children[i]->letter; 
autocomplete(subTrie->children[i], append); }}}
}
int alphabetIndex(char c){
int value;
if (c >= 65 && c <= 90){ //If the character is capital A-Z
value = c - 65;
return value;
}
else if (c >= 97 && c <= 122){ //If the character is lowercase a-z
value = c - 97;
return value + 26;
}
else{
return 0; }
}
char indexAlphabet(int i){
if (i >= 0 && i <= 25){ //If the character is capital A-Z
char c = i + 65;
return c;
}
if (i >= 26 && i <= 51){ //If the character is lowercase a-z
char c = i + 71;
return c;
}
return 0;
}

int main(){
trie_t trie; 
initialize(&trie); 
insert(&trie, "car");
insert(&trie, "dog");
insert(&trie, "train");
insert(&trie, "cat");
insert(&trie, "apple");
insert(&trie, "ball");
insert(&trie, "help");
insert(&trie, "app");
insert(&trie, "hello");
insert(&trie, "toy");
insert(&trie, "world");
insert(&trie,"good");
char searchForWord[25];
int n=0;
printf("ENTER A WORD TO SEARCH FOR:\n");
scanf("%s",searchForWord);
int i;
int isValid = 1;
for (i = 0; i < strlen(searchForWord); i++){
char c = searchForWord[i];
if (!isalpha(c)) {
isValid = 0; } }
if (!isValid) printf("Invalid input! Letters only!\n");
if (isValid) traverse(&trie,searchForWord);
return 0;}