<script>
  import XLSX from "xlsx";

  const SHEET_NAME = "Roles-Permission-Access";

  const ROLE_FEATURE_INDEX = 0;
  const ROLE_NAMES_LENGTH = 3;

  async function processFile({ target: { files } }) {
    const [targetFile] = files;
    const data = await targetFile.arrayBuffer();

    const workbook = XLSX.read(data);
    const targetSheet = workbook.Sheets[SHEET_NAME];
    // console.log(Object.keys(targetSheet["!ref"]));
    const sheetJSON = XLSX.utils.sheet_to_json(targetSheet, {
      raw: true,
      header: 1
    });

    const ROLE_TYPES = sheetJSON[ROLE_FEATURE_INDEX].slice(
      0,
      ROLE_NAMES_LENGTH
    ).map(roleName => roleName.replaceAll("\r\n", " "));

    const FEATURES = sheetJSON[ROLE_FEATURE_INDEX].slice(
      ROLE_NAMES_LENGTH
    ).filter(feature => !!feature);

    console.log(ROLE_TYPES, FEATURES);

    console.log(sheetJSON);
  }

  function convertPermissionToBool(permission) {
    let parsedPermission = permission || "";
    parsedPermission = parsedPermission.toLowerCase();

    return parsedPermission === "Accessible" || parsedPermission === "NA";
  }
</script>

<style>
</style>

<main>
	<label for="file">Input File</label>
	<input type="file" id="file" on:change={processFile}/>
</main>