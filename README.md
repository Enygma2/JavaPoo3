# JavaPoo3
 Atividade03 
      
      package com.mycompany.atividade3;
      
      public class Atividade3 {
          public static void main(String[] args) throws AnoInvalidoException {
              System.out.println("Atividade 03!");
              TestaVeiculos.main(args);
              System.out.println("\n=== TESTE ALTERNATIVO ===");
              Carro meuCarro = new Carro("porsche 911", 2020, 2);
              Moto minhaMoto = new Moto("Titan", 2021, 150);
              
              meuCarro.acelerar(60);
              minhaMoto.acelerar(40);
              
              meuCarro.exibirInformacoes();
              minhaMoto.exibirInformacoes();
          }
      }
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Class Anoinvalido

    package com.mycompany.atividade3;
    
    
    
    public class AnoInvalidoException extends Exception {
        public AnoInvalidoException(String mensagem) {
            super(mensagem);
        }
    }
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Class Carro 

    package com.mycompany.atividade3;
    
    public class Carro extends Veiculo {
        private int numeroPortas;
    
        public Carro(String modelo, int ano, int numeroPortas) throws AnoInvalidoException {
            super(modelo, ano);
            this.numeroPortas = numeroPortas;
        }
    
        public int getNumeroPortas() {
            return numeroPortas;
        }
    
        public void abrirPorta() {
            System.out.println("Porta do carro " + getModelo() + " aberta");
        }
    
        @Override
        public void exibirInformacoes() {
            super.exibirInformacoes();
            System.out.println("Numero de portas: " + numeroPortas);
        }
    }

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Class Moto 

    package com.mycompany.atividade3;
    
    public class Moto extends Veiculo {
        private int cilindradas;
    
        public Moto(String modelo, int ano, int cilindradas) throws AnoInvalidoException {
            super(modelo, ano);
            this.cilindradas = cilindradas;
        }
    
        public int getCilindradas() {
            return cilindradas;
        }
    
        public void empinar() {
            if (getVelocidadeAtual() > 30) {
                System.out.println("Moto " + getModelo() + " empinando!");
            } else {
                System.out.println("Velocidade muito baixa para empinar");
            }
        }
    
        @Override
        public void exibirInformacoes() {
            super.exibirInformacoes();
            System.out.println("Cilindradas: " + cilindradas + "cc");
        }
    }
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Class Simulador de Viagem


    package com.mycompany.atividade3;
    
    public class SimuladorViagem implements Runnable {
        private Veiculo veiculo;
    
        public SimuladorViagem(Veiculo veiculo) {
            this.veiculo = veiculo;
        }
    
        @Override
        public void run() {
            try {
                System.out.println("Viagem do veículo " + veiculo.getModelo() + " começou!");
                
                for (int i = 0; i < 5; i++) {
                    double incremento = 5 + Math.random() * 10; // Entre 5 e 15 km/h
                    veiculo.acelerar(incremento);
                    System.out.println(veiculo.getModelo() + " - Velocidade atual: " + veiculo.getVelocidadeAtual() + " km/h");
                    Thread.sleep(500);
                }
                
                veiculo.frear(veiculo.getVelocidadeAtual()); // Frear completamente
                System.out.println("Viagem do veículo " + veiculo.getModelo() + " terminou!");
                
            } catch (InterruptedException e) {
                System.out.println("Viagem interrompida!");
            } catch (IllegalArgumentException e) {
                System.out.println("Erro durante a viagem: " + e.getMessage());
            }
        }
    }
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Class Testa Veiculos

    package com.mycompany.atividade3;
    
    public class TestaVeiculos {
        public static void main(String[] args) {
            try {
                // Teste com ano válido
                Carro carro = new Carro("Supra", 2020, 2);
                Moto moto = new Moto("CB500", 2023, 500);
                
                System.out.println("=== TESTE INICIAL ===");
                carro.exibirInformacoes();
                System.out.println();
                moto.exibirInformacoes();
                
                // Teste de aceleração
                System.out.println("\n=== TESTE DE ACELERACAO ===");
                try {
                    carro.acelerar(70);
                    moto.acelerar(100);
                } catch (IllegalArgumentException e) {
                    System.out.println("Erro ao acelerar: " + e.getMessage());
                }
                
                // Teste de frenagem
                System.out.println("\n=== TESTE DE FREIO ===");
                try {
                    carro.frear(20);
                    moto.frear(40); // Testando frenagem excessiva
                } catch (IllegalArgumentException e) {
                    System.out.println("Erro ao frear: " + e.getMessage());
                }
                
                // Teste de métodos específicos
                System.out.println("\n=== TESTE DE METODOS ESPECIFICOS ===");
                carro.abrirPorta();
                moto.empinar();
                
                // Teste com ano inválido
                System.out.println("\n=== TESTE ANO INVALIDO ===");
                try {
                    Moto motoAntiga = new Moto("CBX", 1899, 100);
                } catch (AnoInvalidoException e) {
                    System.out.println("Erro ao criar moto: " + e.getMessage());
                }
                
                System.out.println("\n=== SIMULAÇÃO DE VIAGEM COM THREADS ===");
                try {
                    // Resetando velocidades para a simulação
                    carro.frear(carro.getVelocidadeAtual());
                    moto.frear(moto.getVelocidadeAtual());
                    
                    Thread threadCarro = new Thread(new SimuladorViagem(carro));
                    Thread threadMoto = new Thread(new SimuladorViagem(moto));
                    
                    System.out.println("Iniciando viagens...");
                    threadCarro.start();
                    threadMoto.start();
                    
                   
                    threadCarro.join();
                    threadMoto.join();
                    
                    System.out.println("=== STATUS FINAL APÓS VIAGENS ===");
                    carro.exibirInformacoes();
                    System.out.println();
                    moto.exibirInformacoes();
                    
                } catch (InterruptedException e) {
                    System.out.println("Simulação interrompida: " + e.getMessage());
                } catch (IllegalArgumentException e) {
                    System.out.println("Erro durante a simulação: " + e.getMessage());
                }
                
            } catch (AnoInvalidoException e) {
                System.out.println("Erro ao criar veículo: " + e.getMessage());
            }
        }
    }
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Class Veiculos

    package com.mycompany.atividade3;
    
    public abstract class Veiculo {
        protected String modelo;
        protected int ano;
        protected double velocidadeAtual;
    
        public Veiculo(String modelo, int ano) throws AnoInvalidoException {
            if (ano < 1900) {
                throw new AnoInvalidoException("Ano do veículo não pode ser anterior a 1900");
            }
            this.modelo = modelo;
            this.ano = ano;
            this.velocidadeAtual = 0.0;
        }
    
        public String getModelo() {
            return modelo;
        }
    
        public int getAno() {
            return ano;
        }
    
        public double getVelocidadeAtual() {
            return velocidadeAtual;
        }
    
        public void acelerar(double incremento) throws IllegalArgumentException {
            if (incremento < 0) {
                throw new IllegalArgumentException("Incremento de velocidade não pode ser negativo");
            }
            this.velocidadeAtual += incremento;
            System.out.println(modelo + " acelerando para " + velocidadeAtual + " km/h");
        }
    
        public void frear(double decremento) throws IllegalArgumentException {
            if (decremento < 0) {
                throw new IllegalArgumentException("Decremento de velocidade não pode ser negativo");
            }
            if (decremento > velocidadeAtual) {
                throw new IllegalArgumentException("Não pode frear além da velocidade atual");
            }
            velocidadeAtual -= decremento;
            System.out.println(modelo + " freando para " + velocidadeAtual + " km/h");
        }
    
        public void exibirInformacoes() {
            System.out.println("Modelo: " + modelo);
            System.out.println("Ano: " + ano);
            System.out.println("Velocidade atual: " + velocidadeAtual + " km/h");
        }
    }
