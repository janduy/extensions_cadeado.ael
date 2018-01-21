# extensions_cadeado.ael
Cadeado extensions AEL

//--------------> INICIO ATIVA CADEADO

        *55 => {
          NoOp(==========INICIO_ATIVA_CADEADO========);
          NoOp(=========RAMAL:${CALLERID(num)}=======);
          Answer();
          Wait(1);
          Set(Pass=${DB(Senha/${CALLERID(num)})});
          Authenticate(${Pass},d);
          Set(DB(Cadeado/${CALLERID(num)})=1);
          Playback(cabeado-ativo);
          NoOp(==========FIM_ATIVA_CADEADO=======);
        }

        //--------------> FIM ATIVA CADEADO


        //--------------> INICIO DE DESATIVA CADEADO

        *56 => {
          NoOp(=========INICIO_DESATIVA_CADEADO========);
          NoOp(==========RAMAL:${CALLERID(num)}========);
          Answer();
          Wait(1);
          Set(Pass=${DB(Senha/${CALLERID(num)})});
          Authenticate(${Pass},d);
          Set(DB(Cadeado/${CALLERID(num)})=0);
          Playback(cabeado-ativo);
          NoOp(==========FIM_DESATIVA_CADEADO=======);
        }

        //--------------> FIM DE DESATIVA CADEADO


        //--------------> INICIO DE ALTERA SENHA CADEADO

        *57 => {
          NoOp(====INICIO_ALTERA_PASSWORD_CADEADO====);
          Answer();
          Wait(1);
          Set(Pass=${DB(Senha/${CALLERID(num)})});
          Authenticate(${Pass},d);
          Playback(digite-nova-senha);
          Read(NovaSenha,,4,,,);
          Set(DB(Senha/${CALLERID(num)})=${NovaSenha});
          Playback(senha-modificada);
          NoOp(========SENHA ALTERADA:${NovaSenha}=====);
          NoOp(=======FIM_ALTERA_PASSWORD_CADEADO======);
          HangUp();
        }

        //--------------> INICIO DE ALTERA SENHA CADEADO


        //-------------->INICIO MACRO CADEADO
        macro cadeado() {
        NoOp(O valor da tabela é:. ${DB(Cadeado/${CALLERID(num)})});
        Set(cadeado=${DB(Cadeado/${CALLERID(num)})});
        NoOp(O valor da variavel cadeado é:. ${cadeado});
        if ("${cadeado}"=="1") {
          Playback(im-sorry&is-set-to&do-not-disturb&auth-thankyou);
          HangUp();
        }
        NoOp(O cadeado não está ativo, devolvendo a chamada para o contexto original); 
        return;
        //-------------->FIM MACRO CADEADO
