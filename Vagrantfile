# Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  
  config.vm.network "forwarded_port", guest: 5000, host: 5000

  config.vm.provision "shell", inline: <<-SHELL
    
    sudo apt-get update
    sudo apt-get install -y git nano vim python-is-python3 python3-venv python3-pip

    python3 -m venv /home/vagrant/flask_venv
    /home/vagrant/flask_venv/bin/pip install Flask

    sudo tee /etc/systemd/system/flaskapp.service > /dev/null <<EOF
[Unit]
Description=Flask Hello World App
After=network.target

[Service]
User=vagrant
WorkingDirectory=/vagrant
ExecStart=/home/vagrant/flask_venv/bin/flask --app hello run --host=0.0.0.0

[Install]
WantedBy=multi-user.target
EOF

    # Reload systemd, enable and start the service
    sudo systemctl daemon-reload
    sudo systemctl enable flaskapp
    sudo systemctl start flaskapp
  SHELL
end
