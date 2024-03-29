## Token

A lexical token or simply token is a string with an assigned and thus identified meaning. It is structured as a pair consisting of a token name and an optional token value. 

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

## AST Tree

A AST tree or parse tree is an ordered, rooted tree that represents the syntactic structure of a program. A parse tree is made up of tokens (nodes and branches). Nodes can also be referred to as parent nodes and child nodes. A parent node is one which has at least one other node linked by a branch under it. A child node is one which has at least one node directly above it to which it is linked by a branch of the tree. 

``` C++
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

[Lexer](Lexer.md)
