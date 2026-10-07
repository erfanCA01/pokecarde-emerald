all: 00-G000-EN.raw


giftpokemon-%.tx: giftpokemon.asm
	python ../scripts/regionalize.py $< $@ $* $*
giftpokemon-%.o: giftpokemon-%.tx
	../maketools/rgbasm.exe -o $@ $<
giftpokemon-%.gbc: giftpokemon-%.o
	../maketools/rgblink.exe -o $@ $<
giftpokemon1-%.bin: giftpokemon-%.gbc
	python ../scripts/stripgbc.py $< $@
giftpokemon-%.bin: giftpokemon1-%.bin
	python ../scripts/pokemonstructure.py $< $@
giftpokemon-%.mev: giftpokemon-%.bin
	python ../scripts/checksum.py $< $@

prologue-%.tx: prologue.asm
	python ../scripts/regionalize.py $< $@ $* $*
prologue-%.o: prologue-%.tx
	../maketools/rgbasm.exe -o $@ $<
prologue-%.gbc: prologue-%.o
	../maketools/rgblink.exe -o $@ $<
prologue-%.bin: prologue-%.gbc
	python ../scripts/stripgbc.py $< $@

00-G000-%.tx: 00-G000.asm giftpokemon-%.mev prologue-%.bin
	python ../scripts/ereadertext.py $< $@ $*
00-G000-%.o: 00-G000-%.tx
	../maketools/rgbasm.exe -o $@ $<
00-G000-%.gbc: 00-G000-%.o
	../maketools/rgblink.exe -o $@ $<
00-G000-%.z80: 00-G000-%.gbc
	python ../scripts/stripgbc.py $< $@
00-G000-%.vpk: 00-G000-%.z80
	../maketools/nevpk.exe -c -i $< -o $@
00-G000-%.raw: 00-G000-%.vpk
	../maketools/nedcmake.exe -i $< -o $@ -type 1 -region 1
	mv $@-01.raw $@


clean:
	rm -f *.tx *.o *.gbc *.z80 *.bin *.mev *.vpk *.raw