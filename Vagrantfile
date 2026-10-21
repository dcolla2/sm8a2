################################
#### Definició de variables ####
################################

IMATGE_BOX = "debian/bookworm64"
GESTORMV = "virtualbox"

NOM_NODE="sm8act2"

NOM_BASE = "websrv"
ID = "-dacomo" # Canvia-ho pel teu identificador d'alumne
NOM_DOMINI = ".fjeclot.local"

MEMORIA_RAM = 2048
NUM_CPUS = 2

PORT1_GUEST = 80
PORT1_HOST = 18000

PORT2_GUEST = 443
PORT2_HOST = 18443

PORT3_GUEST = 3306
PORT3_HOST = 13306

CARPETA_HOST = "./projectes"
CARPETA_GUEST = "/var/www/html"
PROPIETARI_CARPETA_GUEST = "www-data"
GRUP_CARPETA_GUEST = "www-data"
PERMISOS_CARPETA_GUEST = ["dmode=770", "fmode=660"]

USUARI_VAGRANT = "vagrant"
GRUPS_USUARI_VAGRANT = "www-data"

LLISTAT_PROGRAMARI_BASIC = "net-tools whois aptitude git zip unzip curl gnupg"
LLISTAT_PROGRAMARI_DESENVOLUPADOR = "apache2 apache2-doc mariadb-server mariadb-client php8.2 libapache2-mod-php php8.2-mysql composer php-xml"

############################################
#### Definició de la màquina virtual #######
############################################

Vagrant.configure("2") do |maquina|
	    
    # Definició de paràmetres de la màquina virtual  
	maquina.vm.provider GESTORMV do |parametres|
		parametres.name = NOM_NODE+ID
		parametres.memory = MEMORIA_RAM
		parametres.cpus = NUM_CPUS
		parametres.customize ['modifyvm', :id, '--clipboard', 'bidirectional', '--groups', '/DAW2_SM8']     
	end 
		
	# Definició de paràmetres del sistema de la màquina virtual
	maquina.vm.box = IMATGE_BOX
	maquina.vm.hostname = NOM_BASE+ID+NOM_DOMINI
	maquina.vm.network "forwarded_port", guest: PORT1_GUEST, host: PORT1_HOST
	maquina.vm.network "forwarded_port", guest: PORT2_GUEST, host: PORT2_HOST
	maquina.vm.network "forwarded_port", guest: PORT3_GUEST, host: PORT3_HOST
	maquina.vm.synced_folder CARPETA_HOST, CARPETA_GUEST, owner: PROPIETARI_CARPETA_GUEST, group: GRUP_CARPETA_GUEST, mount_options: PERMISOS_CARPETA_GUEST
	
	### Aprovsionament de programari i configuració###
	maquina.vm.provision "shell", inline: <<-SHELL
		sudo apt-get update -y
		sudo apt-get install -y #{LLISTAT_PROGRAMARI_BASIC}
		sudo apt-get install -y #{LLISTAT_PROGRAMARI_DESENVOLUPADOR}
		gpasswd -a  #{USUARI_VAGRANT} #{GRUPS_USUARI_VAGRANT}											
	SHELL
	
end 
