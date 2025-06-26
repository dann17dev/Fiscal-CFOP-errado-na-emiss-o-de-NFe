## 1. Fiscal: CFOP errado na emissão de NFe

### 🛠 Problema real
Ao emitir notas fiscais eletrônicas (NFe) no Protheus, operadores selecionam CFOPs incorretos para as operações de venda ou devolução. Isso gera rejeições fiscais pela SEFAZ, atrasando o faturamento e gerando retrabalho.

### 📉 Impacto
- Notas rejeitadas pela SEFAZ
- Cancelamentos e reemissões necessárias
- Atraso no processo de faturamento e expedição
- Perda de produtividade

### 💡 Solução aplicada
Criação de uma rotina de validação automática do CFOP no momento da emissão, verificando se o código é compatível com o tipo da operação (Venda ou Devolução). A validação pode ser feita via código ADVPL, exibindo mensagens de erro e impedindo a continuidade até a correção.

### 🧾 Código exemplo (ADVPL)

```advpl
User Function ValidarCFOP()
    Local cTipo := "V"     // "V" = Venda, "D" = Devolução
    Local cCFOP := "1102"  // CFOP informado pelo usuário

    // Validação para Devolução: CFOP deve iniciar com 1, 2, 3 ou 5
    If cTipo == "D" .And. !(SubStr(cCFOP,1,1) $ "1,2,3,5")
        MsgStop("CFOP inválido para devolução. Utilize CFOPs iniciados por 1, 2, 3 ou 5.")
        Return .F.
    EndIf

    // Validação para Venda: CFOP deve iniciar com 5, 6 ou 7
    If cTipo == "V" .And. !(SubStr(cCFOP,1,1) $ "5,6,7")
        MsgStop("CFOP inválido para venda. Utilize CFOPs iniciados por 5, 6 ou 7.")
        Return .F.
    EndIf

    MsgInfo("CFOP válido!")
    Return .T.
Return
