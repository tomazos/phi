# Programming Language - Phi

This document defines the *Phi* programming language.

An *implementation* of the Phi programming language translates an invocation of a Phi program into a JSON document.

An *invocation* of a Phi program consists of a:

- A Phi program
- An ordered sequence of zero or more arguments
- An ordered sequence of zero or more include directories.

A Phi *program* can be specified to the implementation either as a text string or a source file.

A *source file* is a file that shall:

- contain only UTF-8 encoded text
- have a filename stem that is a valid Phi identifier
- have a filename that contains no uppercase letters
- have a file extension of `.phi`

An *include directory* can be any directory.

A source file is parsed according to the following grammar:

    source-file:
        statement-seq_opt
        expression

    object:
        { statement-seq_opt }

    statement-seq:
        statement statement-sep statement-seq
        statement statement-sep_opt
        statement-sep
        access-control: statement-seq
    
    access-control:
        public
        protected
        private

    statement-sep:
        NEWLINE
        ';'

    statement:
        assignment
        args TODO
        abstract IDENTIFIER
        abstract IDENTIFIER = expression
        inherit value
        import-statement
        return expression

    assignment:
        lhs = expression

    expression:
        null
        false
        true
        NUMBER
        STRING
        IDENTIFIER
        array
        object
        expression . IDENTIFIER
        expression binary-operator expression
        unary-operator expression
        expression ( arguments )
        expression ? expression : expression
        expression [ slice ]
        with IDENTIFIER = expression : expression
        for IDENTIFIER in expression : expression
        ( expression )

    arguments:
        expression ,_opt
        arguments , arguments 

    binary-operator: one of
        + - * / % & | == != < > <= >= and or in

    unary-operator: one of
        + - ~ not

    import-statement:
        import qualified-name
        import qualified-name as IDENTIFIER
        from qualified-name import IDENTIFIER
        from qualified-name import IDENTIFIER as IDENTIFIER

Objects have members.

Some statements introduce members.  Assignment statements, inheritance statements and import statements introduce members.

For all names N, there shall be at most one statement introducing a member N.  [Example:

    x = 42
    x = 43 # error multiple definitions of value x
]

An expression that is an IDENTIFIER is a reference.  It refers to a member of an enclosing... TODO

