<%*
	const ea = ExcalidrawAutomate;
	ea.reset();
	const fs = app.vault.adapter.fs;

	let path = tp.file.path(relative=true).slice(0,-3);
	let content = tp.file.content;
	let re = /!\[\[(([\w\d\s\.-]*)\.excalidraw)\]\]/g;
	let matches = [...content.matchAll(re)];
	for (m of matches) {
		let fullname = m[1];
		let head = m[2];
		let f = await ea.createPNG("Excalidraw/" + fullname, 1.3);
		
		var fileReader = new FileReader();
		fileReader.onload = function() {
		  fs.writeFileSync(app.vault.adapter.basePath + "/Files/" + head + ".png", Buffer.from(new Uint8Array(this.result)));
		};
		fileReader.readAsArrayBuffer(f);
	}

	tp.file.move("export/to_export");

	await tp.user.export_python();
	
	tp.file.move(path); %>