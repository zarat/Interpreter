The interpreter finally traverses the AST tree and evaluates branch after branch.

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
