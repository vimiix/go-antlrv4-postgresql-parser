# Antlr v4 Postgresql Golang

> Compiled based on ANTLR 4.13.2

Usage:

## 1. Install

```shell
go get -u github.com/vimiix/go-antlrv4-postgresql-parser
```

## 2. Example

```go
package main

import (
    "fmt"
    "github.com/vimiix/go-antlrv4-postgresql/parser"
    "github.com/antlr4-go/antlr/v4"
)

type TreeShapeListener struct {
    *parser.BasePostgreSQLParserListener
}

func NewTreeShapeListener() *TreeShapeListener {
    return new(TreeShapeListener)
}

func (s *TreeShapeListener) EnterEveryRule(ctx antlr.ParserRuleContext) {
    fmt.Println(ctx.GetText())
}

func main() {

    sql := `SELECT a,b,a<b AS "a<b",a<=b AS "a<=b",a=b AS "a=b",
            a>=b AS "a>=b",a>b AS "a>b",a<>b AS "a<>b" FROM bit_table;`

    input := antlr.NewInputStream(sql)
    lexer := parser.NewPostgreSQLLexer(input)
    stream := antlr.NewCommonTokenStream(lexer, 0)
    p := parser.NewPostgreSQLParser(stream)
    p.AddErrorListener(antlr.NewDiagnosticErrorListener(true))
    p.BuildParseTrees = true
    antlr.ParseTreeWalkerDefault.Walk(NewTreeShapeListener(), p.Root())

}
```

If you want to compile it yourself, please refer to here:

[https://github.com/antlr/grammars-v4/tree/master/sql/postgresql/Go](https://github.com/antlr/grammars-v4/tree/master/sql/postgresql/Go)
