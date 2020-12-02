First we define which symbols we need to represent our tokens.

``` C++
// non terminals
#define EMPTY "EMPTY"
#define SEMI "SEMI" 
#define COMMA "COMMA" 
#define LPAREN "LPAREN" 
#define RPAREN "RPAREN" 
#define LBRACKET "LBRACKET"
#define RBRACKET "RBRACKET" 
#define LBRACE "LBRACE" 
#define RBRACE "RBRACE" 
#define CODE_BLOCK "CODE_BLOCK"

#define ASSIGN "ASSIGN" 
#define PLUS "ADD"
#define INC "INC"
#define MINUS "SUB"
#define DEC "DEC"
#define MUL "MUL"
#define DIV "DIV"
#define MOD "MOD"
#define EQ "EQ"
#define NEQ "NEQ"
#define LT "LT"
#define GT "GT"
#define LEQ "LEQ"
#define GEQ "GEQ"
#define POW "POW"
#define AND "AND"
#define OR "OR"

// terminals
#define IDENTIFIER "IDENTIFIER"
#define VARIABLE "VARIABLE"
#define INTEGER "INTEGER"
```

Then we define the class Token.

``` C++
class Token {

    private:
    
    	std::string type;
    	std::string value; 
              
    public:
    
        Token() {
            type = "";
            value = "";
        } 
           
        Token(std::string _type, std::string _value) {
            type = _type;
            value = _value;
        }  
           
        ~Token(){};	
        
    	std::string _value() { 
            return value; 
        }
        
    	std::string _type() { 
            return type; 
        } 
           
        std::string str() { 
            return ("Token("+type+","+value+")"); 
        }  
          
};
```

And also a class ASTNode.

```
class ASTNode {
    
    public:        

        std::vector<ASTNode*> child;                    
    	Token token;                
        
        ASTNode() {};                        
    	
        ASTNode(Token _token) {
            token = _token;
        }         
        
        ~ASTNode() {}; 
    	
        void make_child(ASTNode _node) {
            ASTNode *temp = new ASTNode(_node._token());
            temp->child = _node.child;
            child.push_back(temp);
        }            
    	
        Token _token() {
            return token;
        }     

        void show(int level) {                

            if(level < 2 && level != 0) 
                std::cout << std::string(level*2, ' ') << "Token('" << token._type() << "', '" << token._value() << "')\n";

            else 
                std::cout << std::string(level*2, ' ') << "Token('" << token._type() << "', '" << token._value() << "')\n";   

            for(auto it = child.begin(); it != child.end(); it++) 
                (*it)->show(level+1);

        }

};
```

Create a [lexer](Lexer.md)
