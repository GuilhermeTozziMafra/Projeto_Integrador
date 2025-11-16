#include <stdio.h>
#include <string.h>

#define MAX_FUNCIONARIOS 150
#define MAX_DIAS 31
#define VALOR_HORA_EXTRA 75.50f
#define LIMITE_DIARIO 3.0f

typedef struct {
    int id;
    char nome[50];
    float horasExtras[MAX_DIAS]; // horas por dia (1-31)
} Funcionario;

int encontrarFuncionarioPorId(Funcionario funcionarios[], int qtd, int id) {
    for (int i = 0; i < qtd; i++) {
        if (funcionarios[i].id == id) {
            return i;
        }
    }
    return -1;
}

void inicializarFuncionarios(Funcionario funcionarios[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        funcionarios[i].id = 0;
        strcpy(funcionarios[i].nome, "");
        for (int d = 0; d < MAX_DIAS; d++) {
            funcionarios[i].horasExtras[d] = 0.0f;
        }
    }
}

void cadastrarFuncionario(Funcionario funcionarios[], int *qtd) {
    if (*qtd >= MAX_FUNCIONARIOS) {
        printf("\nLimite maximo de funcionarios atingido!\n");
        return;
    }

    Funcionario novo;
    printf("\n=== Cadastro de Funcionario ===\n");
    printf("Digite o ID do funcionario: ");
    scanf("%d", &novo.id);
    getchar(); // consumir \n

    if (encontrarFuncionarioPorId(funcionarios, *qtd, novo.id) != -1) {
        printf("Ja existe funcionario com esse ID!\n");
        return;
    }

    printf("Digite o nome do funcionario: ");
    fgets(novo.nome, sizeof(novo.nome), stdin);
    // remove \n do final, se existir
    size_t len = strlen(novo.nome);
    if (len > 0 && novo.nome[len - 1] == '\n') {
        novo.nome[len - 1] = '\0';
    }

    for (int d = 0; d < MAX_DIAS; d++) {
        novo.horasExtras[d] = 0.0f;
    }

    funcionarios[*qtd] = novo;
    (*qtd)++;

    printf("Funcionario cadastrado com sucesso!\n");
}

void registrarHorasExtras(Funcionario funcionarios[], int qtd) {
    if (qtd == 0) {
        printf("\nNao ha funcionarios cadastrados.\n");
        return;
    }

    int id, dia;
    float horas;

    printf("\n=== Registro de Horas Extras ===\n");
    printf("Digite o ID do funcionario: ");
    scanf("%d", &id);

    int indice = encontrarFuncionarioPorId(funcionarios, qtd, id);
    if (indice == -1) {
        printf("Funcionario nao encontrado.\n");
        return;
    }

    printf("Digite o dia (1 a 31): ");
    scanf("%d", &dia);

    if (dia < 1 || dia > MAX_DIAS) {
        printf("Dia invalido.\n");
        return;
    }

    printf("Digite a quantidade de horas extras nesse dia: ");
    scanf("%f", &horas);

    if (horas < 0) {
        printf("Quantidade de horas invalida.\n");
        return;
    }

    // verifica limite diario (somando o que ja existe)
    float somaDia = funcionarios[indice].horasExtras[dia - 1] + horas;
    if (somaDia > LIMITE_DIARIO) {
        printf("Nao e possivel registrar. Limite diario de %.1f horas excedido.\n",
               LIMITE_DIARIO);
        return;
    }

    funcionarios[indice].horasExtras[dia - 1] = somaDia;

    printf("Horas registradas com sucesso para %s no dia %d.\n",
           funcionarios[indice].nome, dia);
}

void listarHorasFuncionario(Funcionario funcionarios[], int qtd) {
    if (qtd == 0) {
        printf("\nNao ha funcionarios cadastrados.\n");
        return;
    }

    int id;
    printf("\n=== Consulta de Horas por Funcionario ===\n");
    printf("Digite o ID do funcionario: ");
    scanf("%d", &id);

    int indice = encontrarFuncionarioPorId(funcionarios, qtd, id);
    if (indice == -1) {
        printf("Funcionario nao encontrado.\n");
        return;
    }

    float totalMes = 0.0f;
    printf("\nHoras extras de %s:\n", funcionarios[indice].nome);
    for (int d = 0; d < MAX_DIAS; d++) {
        if (funcionarios[indice].horasExtras[d] > 0.0f) {
            printf("Dia %2d: %.2f horas\n", d + 1, funcionarios[indice].horasExtras[d]);
        }
        totalMes += funcionarios[indice].horasExtras[d];
    }
    printf("Total de horas extras no mes: %.2f\n", totalMes);
    printf("Valor total a receber: R$ %.2f\n", totalMes * VALOR_HORA_EXTRA);
}

float calcularHorasSemana(Funcionario *f, int semana) {
    int inicio = (semana - 1) * 7;     // 0, 7, 14, 21, 28
    int fim = inicio + 7;              // 7, 14, 21, 28, 35 (mas limitamos a 31)

    if (inicio >= MAX_DIAS) {
        return 0.0f;
    }
    if (fim > MAX_DIAS) {
        fim = MAX_DIAS;
    }

    float total = 0.0f;
    for (int d = inicio; d < fim; d++) {
        total += f->horasExtras[d];
    }
    return total;
}

void fechamentoSemanal(Funcionario funcionarios[], int qtd) {
    if (qtd == 0) {
        printf("\nNao ha funcionarios cadastrados.\n");
        return;
    }

    int semana;
    printf("\n=== Fechamento Semanal ===\n");
    printf("Digite o numero da semana (1 a 5): ");
    scanf("%d", &semana);

    if (semana < 1 || semana > 5) {
        printf("Semana invalida.\n");
        return;
    }

    printf("\nRelatorio de horas extras - Semana %d\n", semana);
    for (int i = 0; i < qtd; i++) {
        float horas = calcularHorasSemana(&funcionarios[i], semana);
        if (horas > 0.0f) {
            float valor = horas * VALOR_HORA_EXTRA;
            printf("ID: %d | Nome: %s | Horas: %.2f | Valor: R$ %.2f\n",
                   funcionarios[i].id, funcionarios[i].nome, horas, valor);
        }
    }
}

void fechamentoMensal(Funcionario funcionarios[], int qtd) {
    if (qtd == 0) {
        printf("\nNao ha funcionarios cadastrados.\n");
        return;
    }

    printf("\n=== Fechamento Mensal ===\n");
    printf("Relatorio de horas extras no mes:\n");

    for (int i = 0; i < qtd; i++) {
        float totalHoras = 0.0f;
        for (int d = 0; d < MAX_DIAS; d++) {
            totalHoras += funcionarios[i].horasExtras[d];
        }
        if (totalHoras > 0.0f) {
            float valor = totalHoras * VALOR_HORA_EXTRA;
            printf("ID: %d | Nome: %s | Horas: %.2f | Valor: R$ %.2f\n",
                   funcionarios[i].id, funcionarios[i].nome, totalHoras, valor);
        }
    }
}

void exibirMenu() {
    printf("\n==============================\n");
    printf("  CONTROLE DE HORAS EXTRAS\n");
    printf("==============================\n");
    printf("1 - Cadastrar funcionario\n");
    printf("2 - Registrar horas extras\n");
    printf("3 - Listar horas de um funcionario\n");
    printf("4 - Fechamento semanal\n");
    printf("5 - Fechamento mensal\n");
    printf("0 - Sair\n");
    printf("Escolha uma opcao: ");
}

int main() {
    Funcionario funcionarios[MAX_FUNCIONARIOS];
    int quantidade = 0;
    int opcao;

    inicializarFuncionarios(funcionarios, MAX_FUNCIONARIOS);

    do {
        exibirMenu();
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                cadastrarFuncionario(funcionarios, &quantidade);
                break;
            case 2:
                registrarHorasExtras(funcionarios, quantidade);
                break;
            case 3:
                listarHorasFuncionario(funcionarios, quantidade);
                break;
            case 4:
                fechamentoSemanal(funcionarios, quantidade);
                break;
            case 5:
                fechamentoMensal(funcionarios, quantidade);
                break;
            case 0:
                printf("\nEncerrando o sistema...\n");
                break;
            default:
                printf("\nOpcao invalida. Tente novamente.\n");
        }

    } while (opcao != 0);

    return 0;
}
