The parser has finally generated a rooted, structured AST tree.

```
Token('CODE_BLOCK', 'CODE_BLOCK')
  Token('ASSIGN', '=')
    Token('VARIABLE', 'a')
    Token('INTEGER', '1')
  Token('ASSIGN', '=')
    Token('VARIABLE', 'b')
    Token('INTEGER', '2')
```

The interpreter traverses the tree and visits each node. If its for example an <code>ASSIGN</code> token, there has to be 2 children - the variable name and the value which should be stored. This is the most important part. It is where variables get created and managed.

```C++
class Interpreter {

	private:

		Parser parser;  

	public:
    
		Interpreter(Parser _parser) {
			parser = _parser;
		} 
    
		~Interpreter() {};

		Token interpret() {	
			ASTNode tree = parser.parse();
			return visit(tree);
		}
        
		Token visit(ASTNode node) {
			if(node._token()._type() == EMPTY) {
				return Token(INTEGER, "0");
			}
            
			else if(node._token()._type() == CODE_BLOCK) { 

				Token token;
                
				for(auto it = node.child.begin(); it != node.child.end(); it++) {

					token = visit(**it);
				
				} 

				return token;

			}
            
			else if(node._token()._type() == INTEGER) {

				return node._token();

			}
            
			else if (node._token()._type() == ASSIGN) {

				Token token = visit(*node.child[1]); 

				if (token._type() == INTEGER) {
                
					int i = 0;
					try {
						i = stoi(token._value());        
					} catch(...) { 
						return Token(INTEGER, "1"); // indicating error                    
					}
					printf("Assigning Integer %d\n", i );
                    
				}

			}
            
			return Token(INTEGER, "0");
               
		}

};
```
