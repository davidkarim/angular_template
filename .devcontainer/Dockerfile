FROM node:12.16.1

# Install Angular
RUN echo N | npm install -g @angular/cli

# Install zsh
RUN apt-get update -qq && apt-get install -y zsh powerline fonts-powerline vim
RUN git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
RUN cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

# Install bat command-line utility
RUN wget https://github.com/sharkdp/bat/releases/download/v0.12.1/bat-musl_0.12.1_amd64.deb
RUN dpkg -i bat-musl_0.12.1_amd64.deb

# Install zsh-autosuggestions command-line utility
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN echo "plugins=(zsh-autosuggestions) \nsource ~/.oh-my-zsh/oh-my-zsh.sh \n" >> ~/.zshrc

# Install envsubst command-line utility
RUN curl -L https://github.com/a8m/envsubst/releases/download/v1.1.0/envsubst-`uname -s`-`uname -m` -o envsubst
RUN chmod +x envsubst
RUN mv envsubst /usr/local/bin

# Install hstr command-line utility
SHELL ["/bin/zsh", "-c"]
RUN echo "deb https://www.mindforger.com/debian stretch main \n" >> /etc/apt/sources.list
RUN wget -qO - https://www.mindforger.com/gpgpubkey.txt | apt-key add -
# Add support for https sources in the sources.list
RUN apt-get install -y apt-transport-https ca-certificates
# Continue installing hstr
RUN apt-get update -qq
RUN apt-get install -y hstr
RUN hstr --show-zsh-configuration >> ~/.hstr
RUN cat ~/.hstr >> ~/.zshrc

# Place history file in working directory so that it persists
RUN sed -i 's/export HISTFILE=~\/.zsh_history/export HISTFILE=$PWD\/.zsh_history/' ~/.zshrc

RUN mkdir /myapp
WORKDIR /myapp
# COPY ./Gemfile /myapp/Gemfile
# COPY ./Gemfile.lock /myapp/Gemfile.lock
# RUN bundle install
COPY . /myapp

# Add a script to be executed every time the container starts.
COPY ./entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 4200

# Start the main process.
CMD ["ng", "serve"]
