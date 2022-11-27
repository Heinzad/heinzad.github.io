Aide Memoire 

How to stack Postfix expressions in Python 
========================================== 

Postfix, or [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation) is ideal for performing maths on a stack. 

Postfix does 'one weird little trick' on our familiar algebra (infix notation) : 

``` 

infix: (10 - 5) / 4 + 8 
postfix: 8 10 5 - 4 / + 

``` 

The upshot is that we can run the postfix expression through stack operations to do the math. 

When given a string of operands and operators in postfix notation:  
* push the operands into the stack until we get an operator, 
* pop twice to retrieve the last two operands, and solve the equation with the operator, 
* push the result into the stack. 

To perform this exercise we will need to use the Stack class we created previously: 

```python 

"""stacking postfix 
Reverse Polish Notation performed on stacks  
Adam Heinz 2022-11-27 

>>>postfix_eval('8 10 5 - 4 / +') 
9.25
>>>postfix_eval('4 24 6 / 2 + * 20 + 2 - 4 +') 
46.0
>>>postfix_eval('10 25 5 9 - - - 60 * 8 + 15 -') 
-1147
""" 

from stack import Stack 

def postfix_math(rpn_operator, rpn_operand1, rpn_operand2): 
    """Performs BODMAS without brackets B 
    on Reverse Polish Notation (RPN) Maths""" 
      
    if rpn_operator == chr(42):             # Multiplication 
        return rpn_operand1 * rpn_operand2 
    
    if rpn_operator == chr(47):             # Division  
        return rpn_operand1 / rpn_operand2 
    
    if rpn_operator == chr(43):             # Addition 
        return rpn_operand1 + rpn_operand2 
    
    else:                                   # Subtraction 
        return rpn_operand1 - rpn_operand2 
    
    

def postfix_eval(postfix_expression): 
    """Stacks and calculates a postfix expression 
    (RPN - Reverse Polish Notation""" 
    
    postfix_stack = Stack() 
    postfix_list = postfix_expression.split() 
    
    for item in postfix_list: 
        
        if item[0] in '0123456789': 
            postfix_stack.push(int(item)) 
                       
        else: 
            operand2 = postfix_stack.pop() 
            operand1 = postfix_stack.pop() 
            rpn_result = postfix_math( item, operand1, operand2) 
            postfix_stack.push(rpn_result) 
            
    return postfix_stack.pop() 
    


# verify the doctests 
if __name__ == '__main__': 
    postfix_eval('8 10 5 - 4 / +') 
    postfix_eval('10 25 5 9 - - - 60 * 8 + 15 -') 
 
# Adam Heinz 2022-11-27  

``` 

Adam Heinz 
27 November 2022 
