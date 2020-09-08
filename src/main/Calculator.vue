<template>
    <div class="calculator">
        <Display :value="displayValue" />
        <Button label="AC" triple @onclick="clearMemory" />
        <Button label="/" operation @onclick="setOperation" />
        <Button label="7" @onclick="addDigit" />
        <Button label="8" @onclick="addDigit" />
        <Button label="9" @onclick="addDigit" />
        <Button label="*" operation @onclick="setOperation" />
        <Button label="4" @onclick="addDigit" />
        <Button label="5" @onclick="addDigit" />
        <Button label="6" @onclick="addDigit" />
        <Button label="-" operation @onclick="setOperation" />
        <Button label="1" @onclick="addDigit" />
        <Button label="2" @onclick="addDigit" />
        <Button label="3" @onclick="addDigit" />
        <Button label="+" operation @onclick="setOperation" />
        <Button label="0" double @onclick="addDigit" />
        <Button label="." @onclick="addDigit" />
        <Button label="=" operation @onclick="setOperation" />
    </div>
</template>
<script>
import Button from "../components/Button"
import Display from "../components/Display"

export default {
    data: function(){
        // Retorna o estado inicial do objeto
        return{
            displayValue: "0",
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current:0
        }
    },
// Referenciando os componentes
    components: { Button, Display },
        methods: {
        // Função para limpar a memória
        clearMemory(){
            Object.assign(this.$data, this.$options.data()) // this.$options.data() = Estado inicial do objeto, está sendo passso para this.$data 
        },
        //Função que seta na calculadora a operação digitada pelo usuário
        setOperation(operation){
            // Primeiro caso, usuário está mexendo no primeiro elemento  
           if(this.current === 0){
               this.operation = operation // A operação será a que o usuário digitar
               this.current = 1 //Passa a digitar no segundo valor
               this.clearDisplay = true // Recebido o segundo dígito o display é limpo 
           }
           else{
            //Segundo caso, a partir da segunda operação setada cai aqui

                //QUando clicado no sinal de = o valor da operação é salvo em equals
                const equals = operation === "="

                const currentOperation = this.operation
                try{
                    // Usando try para tratar possíveis erros do eval
                    this.values[0] = eval(
                        // Usando template string para interpolar os dois índices do array em cima da currtent operation
                        `${this.values[0]} ${currentOperation} ${this.values[1]}`
                    )
                }catch(e){
                    this.$emit('onerror')
                }
                this.values[1] = 0 // Zera o display para setar nova operação

                this.displayValue = this.values[0] // O resultado da operação será colocado no display
                this.operation = equals ? null : operation // se for true === null, se não recebe nova operação
                this.current = equals ? 0 : 1   // Se equals for verdadeiro, o usuário ainda n digitou nenhuma operação, então mexe no índice 0, caso contrário entra no índice 1
                this.clearDisplay = !equals //Limpa o display se for diferente de equals
           }
        },
        //Adiciona dígito recebe n como parâmetro, que é o número digitado
        addDigit(n){
            //Caso haja um . na calculadora, não é permitido add . pela 2ª vez.
            if (n === "." && this.displayValue.includes(".")) {
                //Se já houver . no display apenas sai da função
                return
            }
            // Limpando o display quando = 0 ou qdo for setada como true
             const clearDisplay = this.displayValue === "0" || this.clearDisplay

            // se clearDisplay === true então recebe "", senão recebe o proprio displayValue
            const currentValue = clearDisplay ? "" : this.displayValue

            // Concatenando o current value com o valor 0 ou "" ele coloca o valor digitado
            const displayValue = currentValue + n
            this.displayValue = displayValue           

            // setando clear display para false
             this.clearDisplay = false

             // Alternativa 1
            this.values[this.current] = displayValue
            
            // Alternativa 2
            
            // if (n !== ".") {
            //     // pega o valor corrente [0, 0] no índice 1 ou 2 do array
            //     //Acrescenta o novo valor no índice corrente
            //     const i = this.current
            //     const newValue = parseFloat(displayValue)
            //     this.values[i] = newValue
            // }
        }
    }

}
</script>

<style>
.calculator {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;
    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}
</style>