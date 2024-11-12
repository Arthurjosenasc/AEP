#include <stdio.h>
#include <string.h>
#include <locale.h>

#define MAX_USERS 10

typedef struct {
    char name[50];
    char email[50];
    char password[50];
    int isModerator;
} User;

User users[MAX_USERS];
int userCount = 0;


int menu() {
    int choice;
    printf("\n[ 1 - ENTRAR ]\n[ 2 - CADASTRAR-SE ]\n[ 3 - SAIR ]\nEscolha uma opção: ");
    scanf("%d", &choice);
    return choice;
}


int admin_menu() {
    int choice;
    printf("\n[ 1 - LISTAR USUÁRIOS ]\n[ 2 - ADICIONAR USUÁRIO ]\n[ 3 - ALTERAR USUÁRIO ]\n[ 4 - EXCLUIR USUÁRIO ]\n[ 5 - SAIR ]\nEscolha uma opção: ");
    scanf("%d", &choice);
    return choice;
}


int valid_email(const char* email) {
    return (strchr(email, '@') && strchr(email, '.') > strchr(email, '@'));
}


int valid_senha(const char* senha) {
    int maiuscula = 0, minuscula = 0, numero = 0, especial = 0;
    
    for (int i = 0; senha[i] != '\0'; i++) {
        if (senha[i] >= 'A' && senha[i] <= 'Z') maiuscula = 1;
        if (senha[i] >= 'a' && senha[i] <= 'z') minuscula = 1;
        if (senha[i] >= '0' && senha[i] <= '9') numero = 1;
        if (strchr("!@#$%^&*()-_=+[]{}|;:'\",.<>?/\\`~", senha[i])) especial = 1;
    }
    return (maiuscula && minuscula && numero && especial && strlen(senha) >= 8);
}


void listar_usuarios() {
    if (userCount == 0) {
        printf("Nenhum usuário cadastrado.\n");
    } else {
        printf("Usuários cadastrados:\n");
        for (int i = 0; i < userCount; i++) {
            printf("Nome: %s, E-mail: %s, Moderador: %s\n", users[i].name, users[i].email, users[i].isModerator ? "Sim" : "Não");
        }
    }
}


int nome_existente(const char* nome) {
    for (int i = 0; i < userCount; i++) {
        if (strcmp(users[i].name, nome) == 0) return 1;
    }
    return 0; 
}


void adicionar_usuario() {
    if (userCount >= MAX_USERS) {
        printf("Limite de usuários atingido!\n");
        return;
    }

    User novoUsuario;
    printf("Digite o nome do usuário: ");
    scanf("%s", novoUsuario.name);
    if (nome_existente(novoUsuario.name)) {
        printf("Erro: Nome de usuário já existe. Tente outro nome.\n");
        return;
    }

    printf("Digite o e-mail do usuário: ");
    scanf("%s", novoUsuario.email);
    if (!valid_email(novoUsuario.email)) {
        printf("Erro: E-mail inválido.\n");
        return;
    }

    while (1) {
        printf("Digite a senha do usuário: ");
        scanf("%s", novoUsuario.password);
        if (valid_senha(novoUsuario.password)) break;
        printf("Senha inválida. Deve conter letra maiúscula, minúscula, número e caractere especial.\n");
    }

    novoUsuario.isModerator = 0;
    users[userCount++] = novoUsuario;
    printf("Usuário adicionado com sucesso!\n");
}


void alterar_usuario() {
    char nome[50];
    printf("Digite o nome do usuário a ser alterado: ");
    scanf("%s", nome);

    for (int i = 0; i < userCount; i++) {
        if (strcmp(users[i].name, nome) == 0) {
            printf("Digite o novo nome do usuário: ");
            scanf("%s", users[i].name);
            printf("Digite o novo e-mail do usuário: ");
            scanf("%s", users[i].email);
            printf("Digite a nova senha do usuário: ");
            scanf("%s", users[i].password);
            printf("Usuário alterado com sucesso!\n");
            return;
        }
    }
    printf("Usuário não encontrado!\n");
}


void excluir_usuario() {
    char nome[50];
    printf("Digite o nome do usuário a ser excluído: ");
    scanf("%s", nome);

    for (int i = 0; i < userCount; i++) {
        if (strcmp(users[i].name, nome) == 0) {
            for (int j = i; j < userCount - 1; j++) {
                users[j] = users[j + 1];
            }
            userCount--;
            printf("Usuário excluído com sucesso!\n");
            return;
        }
    }
    printf("Usuário não encontrado!\n");
}


int verif_login(const char* usuario, const char* senha) {
    for (int i = 0; i < userCount; i++) {
        if (strcmp(users[i].name, usuario) == 0 && strcmp(users[i].password, senha) == 0) {
            return i; 
        }
    }
    return -1;
}


void login() {
    char usuario[50], senha[50];
    printf("Usuário: ");
    scanf("%s", usuario);
    printf("Senha: ");
    scanf("%s", senha);

    int index = verif_login(usuario, senha);
    if (index == -1) {
        printf("Usuário não encontrado ou senha incorreta.\n");
        return;
    }

    printf("Bem-vindo, %s!\n", usuario);
    if (users[index].isModerator) {
        int escolha;
        do {
            escolha = admin_menu();
            switch (escolha) {
                case 1: listar_usuarios(); break;
                case 2: adicionar_usuario(); break;
                case 3: alterar_usuario(); break;
                case 4: excluir_usuario(); break;
                case 5: printf("Saindo...\n"); break;
                default: printf("Opção inválida.\n");
            }
        } while (escolha != 5);
    } else {
        printf("Acesso como usuário comum.\n");
    }
}


void registrar() {
    if (userCount >= MAX_USERS) {
        printf("Máximo de usuários atingido!\n");
        return;
    }

    adicionar_usuario();
}

int main() {
    setlocale(LC_ALL, "");
    while (1) {
        int escolha = menu();
        switch (escolha) {
            case 1: login(); break;
            case 2: registrar(); break;
            case 3: printf("Fechando...\n"); return 0;
            default: printf("Opção incorreta.\n");
        }
    }
    return 0;
}
