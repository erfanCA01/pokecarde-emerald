all: 08-O001-EN.raw 08-O002-EN.raw
.SECONDARY: 08-O002-EN.z80

decoration-%.tx: decoration.asm
	python ../scripts/regionalize.py $< $@ $* $*
decoration-%.o: decoration-%.tx
	../maketools/rgbasm.exe -o $@ $<
decoration-%.gbc: decoration-%.o
	../maketools/rgblink.exe -o $@ $<
decoration-%.bin: decoration-%.gbc
	python ../scripts/stripgbc.py $< $@
decoration-%.mev: decoration-%.bin
	python ../scripts/checksum_regi.py $< $@

prologue-%.tx: prologue.asm
	python ../scripts/regionalize.py $< $@ $* $*
prologue-%.o: prologue-%.tx
	../maketools/rgbasm.exe -o $@ $<
prologue-%.gbc: prologue-%.o
	../maketools/rgblink.exe -o $@ $<
prologue-%.bin: prologue-%.gbc
	python ../scripts/stripgbc.py $< $@

08-O001-%.tx: 08-O001.asm decoration-%.mev prologue-%.bin
	python ../scripts/ereadertext.py $< $@ $*
08-O001-%.o: 08-O001-%.tx
	../maketools/rgbasm.exe -o $@ $<
08-O001-%.gbc: 08-O001-%.o
	../maketools/rgblink.exe -o $@ $<
08-O001-%.z80: 08-O001-%.gbc
	python ../scripts/stripgbc.py $< $@
08-O001-%.vpk: 08-O001-%.z80
	../maketools/nevpk.exe -c -i $< -o $@
08-O001-%.raw: 08-O001-%.vpk
	../maketools/nedcmake.exe -i $< -o $@ -type 1 -region 1
	mv $@-01.raw $@

tickets-%.tx: tickets.asm
	python ../scripts/regionalize.py $< $@ $* $*
tickets-%.o: tickets-%.tx
	../maketools/rgbasm.exe -o $@ $<
tickets-%.gbc: tickets-%.o
	../maketools/rgblink.exe -o $@ $<
tickets-%.bin: tickets-%.gbc
	python ../scripts/stripgbc.py $< $@
dtickets-%.mev: tickets-%.bin
	python ../scripts/checksum_regi.py $< $@

08-O002-%.tx: 08-O002.asm tickets-%.mev prologue-%.bin
	python ../scripts/ereadertext.py $< $@ $*
08-O002-%.o: 08-O002-%.tx
	../maketools/rgbasm.exe -o $@ $<
08-O002-%.gbc: 08-O002-%.o
	../maketools/rgblink.exe -o $@ $<
08-O002-%.z80: 08-O002-%.gbc
	python ../scripts/stripgbc.py $< $@
08-O002-%.vpk: 08-O002-%.z80
	../maketools/nevpk.exe -c -i $< -o $@
08-O002-%.raw: 08-O002-%.vpk
	../maketools/nedcmake.exe -i $< -o $@ -type 1 -region 1
	mv $@-01.raw $@

clean:
	rm -f *.tx *.o *.gbc *.z80 *.bin *.mev *.vpk *.raw