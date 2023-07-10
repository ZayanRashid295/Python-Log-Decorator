# Python-Log-Decorator
#It is a python code for log decorator.import math

import os
import random
import re
import sys

def log(descriptor):
    def decorator(func):
        def wrapper(*args, **kwargs):
            # Prepare the log message
            log_message = f"LOG: {func.__name__} ({','.join(map(str, args + tuple(kwargs.values())))})\n"

            # Write the log message to the descriptor
            descriptor.write(log_message)

            # Call the decorated function and get the result
            result = func(*args, **kwargs)

            # Return the result
            return result

        # Return the wrapper function
        return wrapper

    # Return the decorator
    return decorator

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    @log(fptr)
    def my_max(a, b, c):
        return max(a, b, c)

    @log(fptr)
    def my_min(a, b):
        return min(a, b)

    @log(fptr)
    def my_sum(*args):
        return sum(args)

    q = int(input())
    for _ in range(q):
        line = input().split()
        f_name, params = line[0], line[1:]
        if f_name == "my_min":
            res = my_min(*map(int, params))
            fptr.write(f"{res}\n")
        elif f_name == "my_max":
            res = my_max(*map(int, params))
            fptr.write(f"{res}\n")
        elif f_name == "my_sum":
            res = my_sum(*map(int, params))
            fptr.write(f"{res}\n")
        else:
            raise ValueError("Unknown function name %s" % f_name)
    fptr.close()


