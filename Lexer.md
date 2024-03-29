A Lexer (or scanner) takes the sourcecode and chunks it into a series of known words. Each of these words is called a token. Some tokens are single characters (parenthesis or mathematical signs), others may be several characters long (numbers and strings).

```C++
class Lexer {

    private:

        std::string text;
        char current_char;
        int pos;

    public:

        Lexer() {
            text = "";
            pos = 0;
            current_char = EOF;
        }

        Lexer(std::string _text) {
            text = _text;
            pos = 0;
            current_char = text[pos];
        }

        ~Lexer() {};

        void error() {
            std::cout << "lexer: unknown '" << text[pos] << "' at position " << pos << "\n";
            std::exit(0);
        }

        void skip_whitespaces();
        void skip_comments();
        void advance_pos();
        void reduce_pos();
        char peek();
        char peek(int n);
        Token getNextToken();
        Token identifier();
        std::string number();

};

void Lexer::skip_whitespaces() {
    while (text[pos] == ' ' || text[pos] == '\t' || text[pos] == '\n')
        advance_pos();
}

void Lexer::skip_comments() {
    advance_pos();
    advance_pos();
    while (!(current_char == '*' && peek() == '/'))
        advance_pos();
    advance_pos();
    advance_pos();
}

void Lexer::advance_pos() {
    pos++;
    if (pos >= text.length())
        current_char = EOF;
    else
        current_char = text[pos];
}

void Lexer::reduce_pos() {
    pos--;
    if (pos <= 0)
        current_char = EOF;
    else
        current_char = text[pos];
}

std::string Lexer::number() {
    std::string str;
    while (current_char >= 48 && current_char <= 57) {
        str.push_back(current_char);
        advance_pos();
    }
    return str;
}

Token Lexer::identifier() {
    std::string result;
    Token token;
    while ((current_char >= 48 && current_char <= 57) || (current_char >= 65 && current_char <= 90) || (current_char >= 97 && current_char <= 122)) {
        result.push_back(current_char);
        advance_pos();
    }
    token = Token(VARIABLE, result);
    return token;
}

char Lexer::peek() {
    if (pos + 1 >= text.length())
        return EOF;
    else
        return text[pos + 1];
}

char Lexer::peek(int n) {
    if (pos + n >= text.length())
        return EOF;
    else
        return text[pos + n];
}

Token Lexer::getNextToken() {
    std::string temp_str;
    skip_whitespaces();

    while (current_char == '/' && peek() == '*') {
        skip_comments();
    }

    if ((current_char >= 65 && current_char <= 90) || (current_char >= 97 && current_char <= 122)) {
        return identifier();
    } else if (current_char >= 48 && current_char <= 57) {
        temp_str = number();
        return Token(INTEGER, temp_str);
    }

    if (current_char == '[') {
        advance_pos();
        return Token(LBRACKET, "[");
    }

    if (current_char == ']') {
        advance_pos();
        return Token(RBRACKET, "]");
    }

    if (current_char == '^') {
        advance_pos();
        return Token(POW, POW);
    }

    if (current_char == '+') {
        temp_str.push_back(current_char);
        advance_pos();
        return Token(PLUS, temp_str);
    }

    if (current_char == '-') {
        temp_str.push_back(current_char);
        advance_pos();
        return Token(MINUS, temp_str);
    }

    if (current_char == '*') {
        temp_str.push_back(current_char);
        advance_pos();
        return Token(MUL, temp_str);
    }

    if (current_char == '/') {
        temp_str.push_back(current_char);
        advance_pos();
        return Token(DIV, temp_str);
    }

    if (current_char == '%') {
        temp_str.push_back(current_char);
        advance_pos();
        return Token(MOD, temp_str);
    }

    if (current_char == '&' && peek() == '&') {
        temp_str.push_back(current_char);
        advance_pos();
        temp_str.push_back(current_char);
        advance_pos();
        return Token(AND, temp_str);
    }

    if (current_char == '|' && peek() == '|') {
        temp_str.push_back(current_char);
        advance_pos();
        temp_str.push_back(current_char);
        advance_pos();
        return Token(OR, temp_str);
    }

    if (current_char == '(') {
        temp_str.push_back(current_char);
        advance_pos();
        return Token(LPAREN, temp_str);
    }

    if (current_char == ')') {
        temp_str.push_back(current_char);
        advance_pos();
        return Token(RPAREN, temp_str);
    }

    if (current_char == EOF) {
        temp_str.push_back(current_char);
        return Token("EOF", temp_str);
    }

    if (current_char == '=' && peek() == '=') {
        advance_pos();
        advance_pos();
        return Token(EQ, "==");
    }

    if (current_char == '!' && peek() == '=') {
        advance_pos();
        advance_pos();
        return Token(NEQ, "!=");
    }

    if (current_char == '<' && peek() == '=') {
        advance_pos();
        advance_pos();
        return Token(LEQ, "<=");
    }

    if (current_char == '>' && peek() == '=') {
        advance_pos();
        advance_pos();
        return Token(GEQ, ">=");
    }

    if (current_char == '<') {
        advance_pos();
        return Token(LT, "<");
    }

    if (current_char == '>') {
        advance_pos();
        return Token(GT, ">");
    }

    if (current_char == '=' && peek() != '=') {
        Token token(ASSIGN, "=");
        advance_pos();
        return token;
    }

    if (current_char == ';') {
        advance_pos();
        return Token(SEMI, ";");
    }

    if (current_char == ',') {
        advance_pos();
        return Token(COMMA, ",");
    }

    if (current_char == '{') {
        advance_pos();
        return Token(LBRACE, "{");
    }

    if (current_char == '}') {
        advance_pos();
        return Token(RBRACE, "}");
    }

    error();

}
```

The result of the Lexer is a simple list of token.

```
Token('VARIABLE', 'a')
Token('ASSIGN', '=')
Token('INTEGER', '1')
Token('VARIABLE', 'b')
Token('ASSIGN', '=')
Token('INTEGER', '2')
```

[Parser](Parser.md)
