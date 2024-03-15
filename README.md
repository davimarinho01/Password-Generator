<h1>Gerador de senha</h1>

  <h2>Como usar</h2>
  <ul>
    <li>Gere uma senha: Clique no botão "Gerar Nova Senha" ou mude as configurações para gerar uma nova senha aleatória.</li>
    <li>Personalize a senha: Defina o tamanho desejado e selecione as opções de maiúsculas, números e símbolos.</li>
    <li>Copie a senha: Clique no botão "Copiar Senha" para copiar a senha gerada.</li>
  </ul>

  <h1>Sobre o código</h1>

  <p>Esse código HTML é um gerador de senha simples que permite aos usuários gerar senhas aleatórias e personalizá-las. A interface contém três seções principais:</p>

  
  <h3>Título:</h3>
  <br>
  
          <section class="hero">
              <h1 class="title">Gerador de senha</h1>
              <p class="subtitle">Utilize o nosso gerador online para criar uma senha forte e segura</p>
          </section>  

  <h3>Caixa de geração de senha:</h3>
  <br>


          <section class="box">
            <div class="password">
                <div class="text">
                    <input type="text" name="password" id="password"/>
                </div>
                <div class="action">
                    <button id="copy-1">
                        <img src="copy.svg" width="42">
                    </button>
                    <button id="renew">
                        <img src="renew.svg" width="42">
                    </button>
                </div>
            </div>
            <div class="security-indicator">
                <div id="security-indicator-bar" class="bar" ></div>
            </div>
        </section>

  <h3>Caixa de personalização:</h3>
  <br>


        <section class="box customize">
            <h3 class="title">Personalizar</h3>
            <div class="action">
                <div class="password-lenght">
                    <p>Tamanho: <span id="password-lenght-text">16</span></p>
                    <input 
                        type="range" 
                        name="password-lenght" 
                        id="password-lenght" 
                        class="slider"
                        value="16"
                        min="4"
                        max="64"
                    />
                </div>
                <div class="config">
                    <label class="checkbox-container">
                        <span class="text">Maiúsculas</span>
                        <input type="checkbox" id="uppercase-check" checked>
                        <span class="checkmark"></span>
                    </label>
                    <label class="checkbox-container">
                        <span class="text">Números</span>
                        <input type="checkbox" id="number-check" checked>
                        <span class="checkmark"></span>
                    </label>
                    <label class="checkbox-container">
                        <span class="text">Símbolos</span>
                        <input type="checkbox" id="symbol-check" checked>
                        <span class="checkmark"></span>
                    </label>
                </div>                
            </div>
        </section>

  <h3>Botão de copiar senha:</h3>
  <br>


        <div class="submit">
            <button id="copy-2">Copiar senha</button>
        </div>     

  <h1>JavaScript:</h1>
  <br>

        // Declaração das variáveis
        
        let passwordLenght = 16
        
        const inputEl = document.querySelector("#password")
        const upperCaseCheckEl = document.querySelector("#uppercase-check")
        const numberCheckEl = document.querySelector("#number-check")
        const symbolCheckEl = document.querySelector("#symbol-check")
        const securityIndicatorBarEl = document.querySelector("#security-indicator-bar")
        

        /*
          Função para gerar uma senha a partir de caracteres aleatórios e anexar a senha no respectivo input
          do documento HTML
        */ 
        
        function generatePassword() {
            let chars = "abcdefghjkmnpqrstuvwxyz"

            const upperCaseChars ="ABCDEFGHJKLMNPQRSTUVWXYZ"
            const numberChars ="123456789"
            const symbolChars = "?!@&*()[]"

            if(upperCaseCheckEl.checked) {
                chars += upperCaseChars
            }
            
            if(numberCheckEl.checked){
                chars += numberChars
            }
            
            if(symbolCheckEl.checked){
                chars += symbolChars
            }
            

            let password = ""

            for(let i=0; i<passwordLenght; i++) {
                const randomNumber = Math.floor(Math.random() * chars.length)
                password += chars.substring(randomNumber, randomNumber + 1)
            }

            
            inputEl.value = (password)

            calculateQuality() // Chamando a função de verificação de qualidade
            calculateFontSize() // Chamando a função de troca de tamanho de fonte
        }

        
         // Função que determina a qualidade de segurança da senha
        
        
        function calculateQuality() {
        
            // 20% - Critical => 100% - Safe
            
            const percent = Math.round((passwordLenght/64) * 25 + 
                (upperCaseCheckEl.checked ? 15 : 0) + 
                (numberCheckEl.checked ? 25 : 0) +
                (symbolCheckEl.checked ? 35 : 0)
            )

            securityIndicatorBarEl.style.width = `${percent}%` // Alterando o tamanho da barra de amostragem de acordo com o percentual de segurança

            if(percent > 69) {
                //safe
                securityIndicatorBarEl.classList.remove("critical")
                securityIndicatorBarEl.classList.remove("warning")
                securityIndicatorBarEl.classList.add("safe")

            }else if(percent > 50) {
                //warning
                securityIndicatorBarEl.classList.remove("critical")
                securityIndicatorBarEl.classList.add("warning")
                securityIndicatorBarEl.classList.remove("safe")
            }else {
                //critical
                securityIndicatorBarEl.classList.add("critical")
                securityIndicatorBarEl.classList.remove("warning")
                securityIndicatorBarEl.classList.remove("safe")
            }

            if(percent >= 100) {
                securityIndicatorBarEl.classList.add("completed")
            }else {
                securityIndicatorBarEl.classList.remove("completed")
            }
        }

        /*
          Funçao para a verificação do tamanho de caracteres da senha e alteração do tamanho da fonte
          para melhor visualização
        */
        
        function calculateFontSize(){
            if(passwordLenght > 45) {
                inputEl.classList.remove("font-sm")
                inputEl.classList.remove("font-xs")
                inputEl.classList.add("font-xxs")
            } else if (passwordLenght > 32) {
                inputEl.classList.remove("font-sm")
                inputEl.classList.add("font-xs")
                inputEl.classList.remove("font-xxs")
            } else if(passwordLenght > 22) {
                inputEl.classList.add("font-sm")
                inputEl.classList.remove("font-xs")
                inputEl.classList.remove("font-xxs")
            } else {
                inputEl.classList.remove("font-sm")
                inputEl.classList.remove("font-xs")
                inputEl.classList.remove("font-xxs")
            }
        }

        // Função que permite o ícone copiar a senha para a área de transferência
        
        function copy() {
            navigator.clipboard.writeText(inputEl.value)
        }


        // Adicionando o addEventListener nos devidos elementos para acionar suas respectivas funções
        
        const passwordLenghtEl = document.querySelector("#password-lenght")

        passwordLenghtEl.addEventListener("input", () => {
            passwordLenght = passwordLenghtEl.value
            document.querySelector("#password-lenght-text").innerText = passwordLenght
            generatePassword()
        })

        upperCaseCheckEl.addEventListener("click", generatePassword)
        numberCheckEl.addEventListener("click", generatePassword)
        symbolCheckEl.addEventListener("click", generatePassword)

        document.querySelector("#copy-1").addEventListener("click", copy)
        document.querySelector("#copy-2").addEventListener("click", copy)
        document.querySelector("#renew").addEventListener("click", generatePassword)

        
        generatePassword() // Chamada da função generatePassword para que a senha senha atualizada junto à página
