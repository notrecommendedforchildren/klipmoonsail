FROM python:3
USER root
RUN apt update && apt install -y \
 python3-virtualenv python3-dev libffi-dev build-essential \
 libncurses-dev avrdude gcc-avr binutils-avr avr-libc \
 stm32flash dfu-util libnewlib-arm-none-eabi \
 gcc-arm-none-eabi binutils-arm-none-eabi libusb-1.0-0 libusb-1.0-0-dev

RUN mkdir /klipper
ARG UID
ARG GID
RUN groupadd -g ${GID} 3dprinters
RUN useradd klipper --base-dir / -u ${UID} -g ${GID}
RUN chown klipper:3dprinters /klipper
WORKDIR /klipper
USER klipper
COPY klipper ./
ENV PYTHON3DIR="/klipper/klippy-env"
ENV KLIPPER_SRCDIR="/klipper/"

RUN virtualenv -p python3 ${PYTHON3DIR}
RUN ${PYTHON3DIR}/bin/pip install -r ${KLIPPER_SRCDIR}/scripts/klippy-requirements.txt

COPY start-klipper.sh .
USER root 
RUN usermod -a -G dialout klipper
RUN chown -R klipper:3dprinters .
RUN chmod +x start-klipper.sh
CMD ["/klipper/start-klipper.sh"]
