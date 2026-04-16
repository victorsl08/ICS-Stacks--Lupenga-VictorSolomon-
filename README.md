# ICS-Stacks--Lupenga-VictorSolomon-
Infix to postfix to prefix for ICT202
import java.util.Stack;

public class InfixConversion {

    // Function to define precedence
    static int precedence(char ch) {
        switch (ch) {
            case '+':
            case '-': return 1;
            case '*':
            case '/': return 2;
            case '^': return 3;
        }
        return -1;
    }

    // Infix to Postfix
    static String infixToPostfix(String exp) {
        String result = "";
        Stack<Character> stack = new Stack<>();

        for (int i = 0; i < exp.length(); i++) {
            char ch = exp.charAt(i);

            // If operand
            if (Character.isLetterOrDigit(ch)) {
                result += ch;
            }

            // If '('
            else if (ch == '(') {
                stack.push(ch);
            }

            // If ')'
            else if (ch == ')') {
                while (!stack.isEmpty() && stack.peek() != '(') {
                    result += stack.pop();
                }
                stack.pop(); // remove '('
            }

            // Operator
            else {
                while (!stack.isEmpty() && 
                       precedence(stack.peek()) >= precedence(ch)) {
                    result += stack.pop();
                }
                stack.push(ch);
            }
        }

        // Pop remaining operators
        while (!stack.isEmpty()) {
            result += stack.pop();
        }

        return result;
    }

    // Reverse string
    static String reverse(String exp) {
        StringBuilder sb = new StringBuilder(exp);
        return sb.reverse().toString();
    }

    // Infix to Prefix
    static String infixToPrefix(String exp) {
        // Step 1: Reverse
        exp = reverse(exp);

        // Step 2: Replace brackets
        String temp = "";
        for (int i = 0; i < exp.length(); i++) {
            char ch = exp.charAt(i);

            if (ch == '(')
                temp += ')';
            else if (ch == ')')
                temp += '(';
            else
                temp += ch;
        }

        // Step 3: Postfix
        String postfix = infixToPostfix(temp);

        // Step 4: Reverse postfix → prefix
        return reverse(postfix);
    }

    public static void main(String[] args) {
        String infix = "A+B*(C-D)";

        System.out.println("Infix Expression: " + infix);

        String postfix = infixToPostfix(infix);
        System.out.println("Postfix Expression: " + postfix);

        String prefix = infixToPrefix(infix);
        System.out.println("Prefix Expression: " + prefix);
    }
}
