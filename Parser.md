The parser takes the tokens from the lexer one after each other and match it against predefined rules. For example a statement could be

<pre>
// identifier equals string semicolon
name = "Jango";

// function identifier lparen rparen lbrace statement_list rbrace
function add() {  
    a = 1; 
    b = 2; 
}
</pre>

We start always with a statement list because almost everything is a list of statements.

<pre>
class Parser {

    private:

        Lexer lexer;
        Token current_token;   

    public:

        Parser(){};       
        Parser(Lexer _lexer) {
            lexer = _lexer;
            current_token = lexer.getNextToken();
        }     
        ~Parser(){};                    
        void error(std::string str) {
            std::cout<<"parser: "<< str <<"\n";
            std::exit(0);
        }    
        ASTNode parse() {
            ASTNode node = statement_list();
            return node;
        }     
        int consumed = 0; // Todo
        void consume(std::string token_type);

        ASTNode statement_list();
	ASTNode statement();
	ASTNode declare_statement();
	ASTNode assignment_statement();
	ASTNode variable();
        ASTNode factor();      
        ASTNode term();  
        ASTNode expression();     
        ASTNode condition();  

};

void Parser::consume(std::string token_type) {
    if(current_token._type() == token_type) current_token = lexer.getNextToken();
    else error("[parser::consume] unexpected '" + current_token._value() + "', expected '" + token_type + "'\n");
    consumed++;
}

ASTNode Parser::statement_list() {

	ASTNode node(Token(CODE_BLOCK, "CODE_BLOCK"));

	while (current_token._type() != "EOF" and current_token._type() != RBRACKET) {

		node.make_child(statement());

	}

	return node;

}

ASTNode Parser::statement() {

	ASTNode node;

	if (current_token._type() == VARIABLE) {
		node = assignment_statement(); 
	}

	else
		error("[parser::statement] unknown Token '" + current_token._type() + "'");
	
	return node;

}

ASTNode Parser::assignment_statement() {

	ASTNode left = variable();
	consume(ASSIGN);
	ASTNode right;
	right = expression();
	ASTNode node(Token(ASSIGN, "="));
	node.make_child(left);
	node.make_child(right);
	if(current_token._type() == SEMI)
		consume(SEMI);
	return node;
    
}

ASTNode Parser::variable() {

	ASTNode node(current_token);
	consume(VARIABLE);
	return node;
    
}

ASTNode Parser::factor() {
    
	if(current_token._type() == PLUS) {
        ASTNode node(current_token);
        consume(PLUS);
        node.make_child(factor());
        return node;
    } 
	
	else if(current_token._type() == MINUS) {
        ASTNode node(current_token);
        consume(MINUS);
        node.make_child(factor());
        return node;
    } 
	
	else if(current_token._type() == INC) {
        ASTNode node(current_token);
        consume(INC);
        node.make_child(factor());
        return node;
    } 
	
	else if(current_token._type() == DEC) {
        ASTNode node(current_token);
        consume(DEC);
        node.make_child(factor());
        return node;
    } 
	
	else if(current_token._type() == LPAREN) {
        consume(LPAREN);
        ASTNode node = expression();
        consume(RPAREN);
        return node;
    } 
	
	else if(current_token._type() == INTEGER) {
        ASTNode node(current_token);
        consume(INTEGER);
        return node;
    } 
    
	else if (current_token._type() == VARIABLE) {
		ASTNode node(variable());
		return node;
	}

    else 
		error("[parser::factor] unknown Token '" + current_token._type() + "'\n");

}

ASTNode Parser::term() {

    ASTNode node = factor();

    while(true) {      

        ASTNode temp;

        if(current_token._type() == MUL) {
            consume(MUL);
            temp = ASTNode(Token(MUL, "*"));
        } 
		
		else if(current_token._type() == DIV) {
            consume(DIV);
            temp = ASTNode(Token(DIV, "/"));
        } 
		
		else if(current_token._type() == MOD) {
            consume(MOD);
            temp = ASTNode(Token(MOD, "%"));
        } 
		
		else if(current_token._type() == POW) {
            consume(POW);
            temp = ASTNode(Token(POW, "^"));
        } 

        else 
			break;

        temp.make_child(node);
        temp.make_child(factor());
        node = temp;

    }

    return node;

}
  
ASTNode Parser::expression() {

    ASTNode node = term(); 

    while(true) {

        ASTNode temp;
        
		if(current_token._type() == PLUS) {
            consume(PLUS);
            temp = ASTNode(Token(PLUS, "+"));
        } 
		
		else if(current_token._type() == MINUS) {
            consume(MINUS);
            temp = ASTNode(Token(MINUS, "-"));
        } 
		
		else 
			break; 

        temp.make_child(node);
        temp.make_child(term());
        node = temp;

    }

    return node;

}

ASTNode Parser::condition() {

    ASTNode node;

    ASTNode left = expression(); 

    if(current_token._type() == EQ) {
        node = Token(EQ, EQ);
        consume(EQ);
    } 
	
	else if(current_token._type() == NEQ) {
        node = Token(NEQ, NEQ);
        consume(NEQ);
    } 
	
	else if(current_token._type() == LT) {
        node = Token(LT, LT);
        consume(LT);
    } 
	
	else if(current_token._type() == GT) {
        node = Token(GT, GT);
        consume(GT);
    } 
	
	else if(current_token._type() == LEQ) {
        node = Token(LEQ, LEQ);
        consume(LEQ);
    } 
	
	else if(current_token._type() == GEQ) {
        node = Token(GEQ, GEQ);
        consume(GEQ);
    } 
	
	else if(current_token._type() == AND) {
        node = Token(AND, AND);
        consume(AND);
    } 
	
	else if(current_token._type() == OR) {
        node = Token(OR, OR);
        consume(OR);
    }  

    else 
		error("[parser::condition] unknown Token '" + current_token._type() + "'");

    node.make_child(left); 
    node.make_child(expression()); 

    return node;

}
</pre>
