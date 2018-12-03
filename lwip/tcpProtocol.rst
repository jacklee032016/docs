TCP Protocol and its Implementation
#########################################
Dec.2nd, 2018	Jack Lee



3-way handshake
-----------------

::

          client  --> SYN(port) -->  server
   (ESTABLISHED)  <-- SYN ACK <--   
                  --> ACK -->        (ESTABLISHED)          

Connection terminate
-----------------------
4 messages to terminal duplicated direction of connection:

::

  Peer(FIN_WAIT_1)    --> FIN -->    Peer (CLOSE_WAIY)
      (FIN_WAIT_2)    <-- ACK <--     
        
  Peer(TIME_WAIT)     <-- FIN <--    Peer (LAST_ACK)
                      --> ACK -->    (CLOSE)
          
Data packet
------------------

::

   Peer     --> Data(PSH) -->   Peer
            <-- ACK       <--   Peer
