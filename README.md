# Clearwater Docker Compose file

Created a new docker compose file which will support affinity and anti-affinity policies for a set of 2 clearwater services.

##Affinity Policy
We wanted the two clearwater services- ralf and sprout to run on the same node. So we had to set the environment parameter for ralf  to have "affinity==sprout" and vice a versa for sprout "affinity==ralf" 

    sprout:
        image: 10.27.128.8/nfv/clearwaterdocker_sprout
        networks:
          default:
            aliases:
              - scscf.sprout
              - icscf.sprout
        ports:
          - 22
        environment:
          - "affinity:container==~clearwater_ralf*"


    ralf:
        image: 10.27.128.8/nfv/clearwaterdocker_ralf
        ports:
          - 22
        environment:
          - "affinity:container==~clearwater_sprout*"



##Anti-Affinity Policy
We wanted bono and ellis to run on two different nodes. So we had to set the environment key for bono to have "affinity!=ellis" and vice a versa for ellis "affinity!=bono" 

     bono:
        image: 10.27.128.8/nfv/clearwaterdocker_bono
        ports:
          - 22
          - "5062:5062"
        expose:
          - "3478"
          - "3478/udp"
          - "5060"
          - "5060/udp"
        environment:
          - "affinity:container!=~clearwater_ellis*"
          
    ellis:
        image: 10.27.128.8/nfv/clearwaterdocker_ellis
        ports:
          - 22
          - "80:80"
        environment:
          - "affinity:container!=~clearwater_bono*"


####Note
The image repository, "10.27.128.8/nfv" is the DTR repo that we have setup up locally and tagged it for each clearwater conatiner images(e.g clearwaterdocker_bono, clearwaterdocker_ellis etc)
