# CRIANDO-UM-DNS-RESOVLVER-EM-C
O código abaixo mostra como criar um programa em C que recebe um domínio e retorna o endereço IP, além de demonstrar como varrer subdomínios usando um arquivo .txt.

⚠️ Uso exclusivo para fins educacionais • Desenvolvido por M!ss s3c

📝 Guia passo a passo: Criando e executando seu programa

1. Criar o arquivo do programa

No terminal, digite:

nano resover.c


Adicione o seguinte código básico:

#include <stdio.h>
#include <netdb.h>    
#include <arpa/inet.h>

int main(int argc, char *argv[]){
    if(argc <= 1){
        printf("Modo de uso: ./resover www.nome do site aqui.com.br\n");
        return 0;
    } else {
        struct hostent *alvo = gethostbyname(argv[1]);
        if(alvo == NULL){
            printf("Ocorreu um erro :( \n");
        } else {
            printf("IP: %s\n", inet_ntoa(*((struct in_addr *)alvo->h_addr)));
        }
    }
}

2. Compilar o programa

No terminal, digite:

gcc resover.c -o resover


gcc → compilador C.

-o resover → nome do executável.

3. Executar o programa
./resover www.nome do site aqui.com.br


Saída esperada:

IP: 192.168.0.1

4. Criando um scanner de subdomínios

Você pode usar um arquivo .txt contendo possíveis subdomínios e varrer a rede:

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <sys/types.h>
#include <arpa/inet.h>
#include <netdb.h>

int main(int argc, char *argv[])
{
    char *alvo;
    struct hostent *host;
    char *result;
    char txt[50];
    FILE *rato;

    if(argc < 3){
        printf("------------------------------------------------\n");
        printf("--------- DNSRATO V1.0 --------\n");
        printf("Uso: ./dnsrato alvo.com.br rato.txt\n");
        printf("M!ss s3c -- ig:@m!sss3c\n");
        printf("------------------------------------------------\n");
        return 0;
    }

    alvo = argv[1];
    rato = fopen(argv[2],"r");
    if(rato == NULL){
        printf("Erro ao abrir o arquivo de subdomínios.\n");
        return 1;
    }

    while(fscanf(rato, "%s", txt) != EOF){
        result = (char *) strcat(txt, alvo);
        host = gethostbyname(result);
        if(host == NULL){
            continue;
        }
        printf("HOST ENCONTRADO: %s =====> IP: %s \n", result, inet_ntoa(*(struct in_addr *)host->h_addr));
    }

    fclose(rato);
}

5. Executar o scanner de subdomínios
./dnsrato alvo.com.br subdominios.txt


subdominios.txt → lista de possíveis subdomínios, ex: admin, mail, ftp.

Saída esperada:

HOST ENCONTRADO: admin.alvo.com.br =====> IP: 192.168.0.10
HOST ENCONTRADO: ftp.alvo.com.br =====> IP: 192.168.0.11

👉 O que você aprendeu aqui

gethostbyname() → converte um domínio em endereço IP.

inet_ntoa() → converte IP interno em formato legível (ASCII).

Arquivos .txt → permitem criar listas de subdomínios para varredura.

Validação de entrada → checa se argumentos estão corretos antes de executar.

Loops e concatenação de strings → permitem automatizar varredura de subdomínios.

Avisos importantes → esse tipo de varredura deve ser usado apenas em ambientes autorizados e para fins educacionais.
