#! /usr/bin/python3

import os
import sys

def HexDump():
    if len(sys.argv) != 3:
         print("\nNo input/output file is provided!")
         print("       \uFFEC   ")
         print("usage : python3 " + str(sys.argv[0]) + " [filename/filepath] [outfile]\n")
         exit(1)

    if not os.path.isfile(sys.argv[1]):
         print("\nFile does not exist!\nPlease provide a valid file name.\n")
         exit(1)

    file_path = sys.argv[1]       ##gets the filename/filepath as input(relative or absolute)
    out_path = sys.argv[2]        ##gets the output filename/filepath as input(relative or absolute)

    try:
        with open(os.path.join(os.path.dirname(__file__), file_path), 'rb') as infile:     ##'rb' - open the file in binary format 
            with open(os.path.join(os.path.dirname(__file__), out_path), 'w') as outfile:
                data = infile.read(16)     ##read data 16 bytes at once
                index = 0

                while len(data) != 0:

                    offset = format(index, '08x')         ##display the offset in hexadecimal format
                    print(offset, end = ": ")
                    print(offset, end = ": ", file=outfile)     ##write to the outfile
                    index += 16

                    hexvals = format_to_hex(data)         ##format the read data into hexadecimal values

                    if len(hexvals) != 32:
                        s = "".ljust(32-len(hexvals), " ")
                        hexvals += s

                    hex_list = []
                    for i in range(0, len(hexvals), 4):   ##split the hex values into 4-digit groups 
                        hex_list.append(hexvals[i:i+4])
    
                    print(*hex_list, sep =" ", end = "  ")
                    print(*hex_list, sep =" ", end = "  ", file=outfile)

                    text = format_to_ascii(data)          ##ascii representation of the read data
                    print(text)
                    print(text, file=outfile)

                    data = infile.read(16)

    except Exception as e:
            print("Exception Ocuured! : "+ str(e))        ##displays if any exception is occurred


def format_to_hex(data):
    hexvals = "" 
    for i in data:
        hexvals += format(i, '02x')

    return hexvals 


def format_to_ascii(data):
    text = ""
    for j in data:
        if j <= 127 and j >= 32:        ##checks if the characters are printable(ASCII printable characters --> character code 32-127)
            text += chr(j)              ##chr() --> converts a number into it’s ASCII character
        else:
            text += "."                 ##non-printable characters display as "."

    return text

HexDump()
